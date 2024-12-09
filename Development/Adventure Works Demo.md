# Adventure Works Demo

Sunday, December 02, 2012\
3:23 AM

```Console
cd \NotBackedUp\AdventureWorks\
```

## Forward Integration

### Merge latest version of \$/AdventureWorks/Main branch into Dev/VS2008LT branch

```Console
tf merge /recursive Main Dev\VS2008LT
```

### Discard merge from \$/AdventureWorks/Main branch into Dev/VS2008LT branch

```Console
tf merge /discard /recursive Main Dev\VS2008LT
```

## Reverse Integration

### Merge latest version of \$/AdventureWorks/Dev/VS2008LT branch into Main branch

```Console
tf merge /recursive Dev\VS2008LT Main
```

### Discard merge from \$/AdventureWorks/Dev/VS2008LT branch into Main branch

```Console
tf merge /discard /recursive Dev\VS2008LT Main
```
