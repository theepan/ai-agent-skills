# JVM Performance Antipatterns

Quick reference for common Java performance issues in code review.

## Table of Contents
1. [Memory & GC](#memory--gc)
2. [Collections](#collections)
3. [Strings](#strings)
4. [Database](#database)
5. [Concurrency](#concurrency)
6. [I/O](#io)

## Memory & GC

### Object Creation in Loops
```java
// SLOW - Creates new DateTimeFormatter per iteration
for (LocalDate date : dates) {
    DateTimeFormatter fmt = DateTimeFormatter.ofPattern("yyyy-MM-dd");
    results.add(date.format(fmt));
}

// FAST - Reuse immutable formatter
private static final DateTimeFormatter FMT = DateTimeFormatter.ofPattern("yyyy-MM-dd");
for (LocalDate date : dates) {
    results.add(date.format(FMT));
}
```

### Autoboxing in Hot Paths
```java
// SLOW - Autoboxing creates objects
Map<Integer, Integer> map = new HashMap<>();
for (int i = 0; i < 1_000_000; i++) {
    map.put(i, i * 2);  // Boxing int -> Integer
}

// FAST - Use primitive collections (Eclipse Collections, Trove, fastutil)
IntIntMap map = new IntIntHashMap();
```

### Finalizers & Cleaners
```java
// AVOID - Finalizers delay GC, unpredictable
@Override protected void finalize() { cleanup(); }

// BETTER - try-with-resources or explicit close()
try (Resource r = new Resource()) { ... }
```

## Collections

### Wrong Collection Type
```
Use Case                  | Best Choice
--------------------------|----------------------------------
Frequent random access    | ArrayList
Frequent insert/remove    | LinkedList (rarely), ArrayDeque
Unique elements           | HashSet
Sorted unique elements    | TreeSet
Key-value lookup          | HashMap
Sorted key-value          | TreeMap
Thread-safe map           | ConcurrentHashMap
FIFO queue                | ArrayDeque
```

### Pre-sizing Collections
```java
// SLOW - Multiple array resizes
List<String> list = new ArrayList<>();
for (int i = 0; i < 10000; i++) list.add(items[i]);

// FAST - Pre-size when size is known
List<String> list = new ArrayList<>(10000);
```

### Iterating with Index Over LinkedList
```java
// O(n^2) - get(i) traverses from head each time
for (int i = 0; i < linkedList.size(); i++) {
    process(linkedList.get(i));
}

// O(n) - Use iterator
for (String item : linkedList) {
    process(item);
}
```

## Strings

### Concatenation in Loops
```java
// O(n^2) - Creates new String each iteration
String result = "";
for (String s : items) {
    result += s + ",";
}

// O(n) - StringBuilder
StringBuilder sb = new StringBuilder();
for (String s : items) {
    sb.append(s).append(",");
}

// CLEANEST - String.join or Collectors
String result = String.join(",", items);
```

### Unnecessary String Operations
```java
// WASTEFUL
if (str.toLowerCase().equals("test"))  // Creates new String

// BETTER
if (str.equalsIgnoreCase("test"))

// WASTEFUL
str.replaceAll("\\.", "_")  // Regex overhead

// BETTER for literal replacement
str.replace(".", "_")
```

## Database

### N+1 Query Problem
```java
// N+1 - 1 query for orders + N queries for items
List<Order> orders = orderRepo.findAll();
for (Order o : orders) {
    List<Item> items = itemRepo.findByOrderId(o.getId());  // N queries!
}

// FETCH JOIN - 1 query
@Query("SELECT o FROM Order o JOIN FETCH o.items")
List<Order> findAllWithItems();

// Or use @EntityGraph
@EntityGraph(attributePaths = {"items"})
List<Order> findAll();
```

### Missing Indexes
Review queries for:
- WHERE clauses on non-indexed columns
- JOIN conditions without indexes
- ORDER BY on non-indexed columns
- LIKE with leading wildcard `LIKE '%term'` (can't use index)

### Fetching Too Much Data
```java
// Fetches all columns
@Query("SELECT u FROM User u WHERE u.active = true")
List<User> findActive();

// Projection - only needed columns
@Query("SELECT new com.app.UserSummary(u.id, u.name) FROM User u WHERE u.active = true")
List<UserSummary> findActiveSummaries();
```

## Concurrency

### Excessive Synchronization
```java
// BLOCKING - Entire method synchronized
public synchronized void process(Request r) {
    validate(r);     // Doesn't need sync
    updateState(r);  // Needs sync
    notify(r);       // Doesn't need sync
}

// BETTER - Minimize critical section
public void process(Request r) {
    validate(r);
    synchronized(this) { updateState(r); }
    notify(r);
}
```

### Lock Contention Patterns
```java
// Single lock bottleneck
private final Object lock = new Object();

// Striped locking for better throughput
private final Object[] locks = new Object[16];  // Or use Striped from Guava
private Object getLock(String key) {
    return locks[Math.abs(key.hashCode() % locks.length)];
}
```

### Thread-Unsafe Patterns
```java
// NOT THREAD-SAFE
private SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
// SimpleDateFormat is not thread-safe!

// THREAD-SAFE options
private static final DateTimeFormatter FMT = DateTimeFormatter.ofPattern("yyyy-MM-dd");
// Or ThreadLocal<SimpleDateFormat>
```

## I/O

### Unbuffered Streams
```java
// SLOW - Unbuffered, one byte at a time to OS
FileInputStream fis = new FileInputStream(file);
while ((b = fis.read()) != -1) { ... }

// FAST - Buffered reads
BufferedInputStream bis = new BufferedInputStream(new FileInputStream(file));
// Or use Files.newBufferedReader/Writer
```

### Not Using NIO for Large Files
```java
// Loads entire file into memory
byte[] data = Files.readAllBytes(path);

// Stream processing for large files
try (Stream<String> lines = Files.lines(path)) {
    lines.filter(...).forEach(...);
}

// Memory-mapped for random access
try (FileChannel fc = FileChannel.open(path)) {
    MappedByteBuffer mbb = fc.map(READ_ONLY, 0, fc.size());
}
```

### Resource Leaks
```java
// LEAK - Connection not closed on exception
Connection conn = dataSource.getConnection();
// ... exception thrown here
conn.close();  // Never reached

// SAFE - try-with-resources
try (Connection conn = dataSource.getConnection();
     PreparedStatement ps = conn.prepareStatement(sql)) {
    // Automatically closed
}
```
