# Incorrect use of the `find` method

```
  let agencyValue = "-- All --";
    agencyList.find((item: any) => {
      if (
        item.agencyId ===
        parseInt(searchParams.get("agencyId")?.toString() || "0")
      ) {
        agencyValue = item.agencyName;
      }
    });
```

### Path to the bug: UserList.tsx

## Possible fix

The find method returns the value of the first element in the provided array that satisfies the provided testing function. So, we should use the return value of the find method instead of using a variable to store the value.

```
  const agencyValue = agencyList.find((item: any) => {
      if (
        item.agencyId ===
        parseInt(searchParams.get("agencyId")?.toString() || "0")
      ) {
        return item.agencyName;
      }
    }) || "-- All --";
```
