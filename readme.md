# Issue with `coverlet` tool

When used to generate a report with an EntityFramework enum that uses a default value, `coverlet` skips the Source repo and the report is incorrectly generated.

How to test:

1. install coverlet (v1.4.1.0)
2. clone this repo
3. run `dotnet build` in root (v2.1.505 of dotnet tool)
4. run `coverlet ./Tests/bin/Debug/netcoreapp2.1/Tests.dll --target "dotnet" --targetargs "test ./Tests/Tests.csproj --no-build" --format opencover`

Expected output:

```
+--------+--------+--------+--------+
| Module | Line   | Branch | Method |
+--------+--------+--------+--------+
| Source | 0%     | 100%   | 0%     |
+--------+--------+--------+--------+

+---------+--------+--------+--------+
|         | Line   | Branch | Method |
+---------+--------+--------+--------+
| Total   | 0%     | 100%   | 0%     |
+---------+--------+--------+--------+
| Average | 0%     | 100%   | 0%     |
+---------+--------+--------+--------+
```

Actual output:

```
+--------+--------+--------+--------+
| Module | Line   | Branch | Method |
+--------+--------+--------+--------+

+---------+--------+--------+--------+
|         | Line   | Branch | Method |
+---------+--------+--------+--------+
| Total   | 100%   | 100%   | 100%   |
+---------+--------+--------+--------+
| Average | ∞%     | ∞%     | ∞%     |
+---------+--------+--------+--------+
```

The issue appears because of this code:


```
public void SimpleMethod(EntityState entityState = EntityState.Added)
{

}
```

If the `entityState` var does not have a default value, then everything works as expected:

```
public void SimpleMethod(EntityState entityState)
{

}
```
