# Optional
## 什么
- null的容器对象
- 可以保存 T 或 null

## 作用：
- 解决空指针异常

## 常用方法：
- 返回empty值： static <T> Optional<T> empty()
- 构造Optional
  - static <T> Optional<T> of(T value)
  - static <T> Optional<T> ofNullable(T value)
- 值相等判断：  boolean equals(Object obj)
- 值存在，并且匹配predicate，返回值,否则Empty:
  - Optional<T> filter(Predicate<? super <T> predicate)
- 值存在，返回映射方法的值,否则Empty:  
  - <U> Optional<U> flatMap(Function<? super T,Optional<U>> mapper)
- 返回值: T get()
- 是否存在: isPresent()
- 存在则调用Consumer: ifPresent(Consumer<? super T> consumer)
- 不存在值返回other： orElse(T other)
- 存在该值，返回值， 否则返回other 调用的结果
  - T orElseGet(Supplier<? extends T> other)
- 存在该值，返回包含的值，否则抛出由 Supplier 继承的异常
  - <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier)
