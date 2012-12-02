# Northwind Demo

Sunday, December 02, 2012
3:23 AM

```Console
cd \NotBackedUp\Northwind\
```

**Forward Integration**

Merge latest version of \$/Northwind/Main branch into Dev/VS2008 branch

```Console
tf merge /recursive Main Dev\VS2008
```

Discard merge from \$/Northwind/Main branch into Dev/VS2008 branch

```Console
tf merge /discard /recursive Main Dev\VS2008
```

**Reverse Integration**

Merge latest version of \$/Northwind/Dev/VS2008 branch into Main branch

```Console
tf merge /recursive Dev\VS2008 Main
```

Discard merge from \$/Northwind/Dev/VS2008 branch into Main branch

```Console
tf merge /discard /recursive Dev\VS2008 Main
```

---


```Console
cd \NotBackedUp\Northwind\
```

**Forward Integration**

Merge latest version of \$/Northwind/Main branch into Dev/VS2010 branch

```Console
tf merge /recursive Main Dev\VS2010
```

Discard merge from \$/Northwind/Main branch into Dev/VS2010 branch

```Console
tf merge /discard /recursive Main Dev\VS2010
```

**Reverse Integration**

Merge latest version of \$/Northwind/Dev/VS2010 branch into Main branch

```Console
tf merge /recursive Dev\VS2010 Main
```

Discard merge from \$/Northwind/Dev/VS2010 branch into Main branch

```Console
tf merge /discard /recursive Dev\VS2010 Main
```
