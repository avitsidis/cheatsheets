# Scala linq equivalence

```
xs.Aggregate(accumFunc) -> xs.reduceLeft(accumFunc)
xs.Aggregate(seed, accumFunc) -> xs.foldLeft(seed)(accumFunc)
xs.Aggregate(seed, accumFunc, trans) -> trans(xs.foldLeft(seed)(accumFunc))
xs.All(pred) -> xs.forall(pred)
xs.Any() -> xs.nonEmpty
xs.Any(pred) -> xs.exists(pred)
xs.AsEnumerable() -> xs.asTraversable // roughly
xs.Average() -> xs.sum / xs.length
xs.Average(trans) -> trans(xs.sum / xs.length)
xs.Cast<A>() -> xs.map(_.asInstanceOf[A])
xs.Concat(ys) -> xs ++ ys
xs.Contains(x) -> xs.contains(x) //////
xs.Contains(x, eq) -> xs.exists(eq(x, _))
xs.Count() -> xs.size
xs.Count(pred) -> xs.count(pred)
xs.DefaultIfEmpty() -> if(xs.isEmpty) List(0) else xs // Use `mzero` (from Scalaz) instead of 0 for more genericity
xs.DefaultIfEmpty(v) -> if(xs.isEmpty) List(v) else xs
xs.Distinct() -> xs.distinct
xs.ElementAt(i) -> xs(i)
xs.ElementAtOrDefault(i) -> xs.lift(i).orZero // `orZero` is from Scalaz
xs.Except(ys) -> xs.diff(ys)
xs.First() -> xs.head
xs.First(pred) -> xs.find(pred) // returns an `Option`
xs.FirstOrDefault() -> xs.headOption.orZero
xs.FirstOrDefault(pred) -> xs.find(pred).orZero
xs.GroupBy(f) -> xs.groupBy(f)
xs.GroupBy(f, g) -> xs.groupBy(f).mapValues(_.map(g))
xs.Intersect(ys) -> xs.intersect(ys)
xs.Last() -> xs.last
xs.Last(pred) -> xs.reverseIterator.find(pred) // returns an `Option`
xs.LastOrDefault() -> xs.lastOption.orZero
xs.LastOrDefault(pred) -> xs.reverseIterator.find(pred).orZero
xs.Max() -> xs.max
xs.Max(f) -> xs.maxBy(f)
xs.Min() -> xs.min
xs.Min(f) -> xs.minBy(f)
xs.OfType<A>() -> xs.collect { case x: A => x }
xs.OrderBy(f) -> xs.sortBy(f)
xs.OrderBy(f, comp) -> xs.sortBy(f)(comp) // `comp` is an `Ordering`.
xs.OrderByDescending(f) -> xs.sortBy(f)(implicitly[Ordering[A]].reverse)
xs.OrderByDescending(f, comp) -> xs.sortBy(f)(comp.reverse)
Enumerable.Range(start, count) -> start until start + count
Enumerable.Repeat(x, times) -> Iterator.continually(x).take(times)
xs.Reverse() -> xs.reverse
xs.Select(trans) -> xs.map(trans) // For indexed overload, first `zipWithIndex` and then `map`.
xs.SelectMany(trans) -> xs.flatMap(trans)
xs.SequenceEqual(ys) -> xs.sameElements(ys)
xs.Skip(n) -> xs.drop(n)
xs.SkipWhile(pred) -> xs.dropWhile(pred)
xs.Sum() -> xs.sum
xs.Sum(f) -> xs.map(f).sum // or `xs.foldMap(f)`. Requires Scalaz.
xs.Take(n) -> xs.take(n)
xs.TakeWhile(pred) -> xs.takeWhile(pred)
xs.OrderBy(f).ThenBy(g) -> xs.sortBy(x => (f(x), g(x))) // Or: xs.sortBy(f &&& g). `&&&` is from Scalaz.
xs.ToArray() -> xs.toArray // Use `xs.toIndexedSeq` for immutable indexed sequence.
xs.ToDictionary(f) -> xs.map(f.first).toMap // `first` is from Scalaz. When f = identity, you can just write `xs.toMap`.
xs.ToList() -> xs.toList // This returns an immutable list. Use `xs.toBuffer` if you want a mutable list.
xs.Union(ys) -> xs.union(ys)
xs.Where(pred) -> xs.filter(pred)
xs.Zip(ys, f) -> (xs, ys).zipped.map(f) // When f = identity, use `xs.zip(ys)`.
```

## Sources
- https://stackoverflow.com/questions/8104846/chart-of-ienumerable-linq-equivalents-in-scala