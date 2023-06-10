# We shouldn't but conditions inside the dependency array of useEffect

```
useEffect(() => {
    dispatch(
      updateElementValueAgencyListReducer({
        elementName: "agencyList",
        value: agenciesForCounty,
      })
    );
    dispatch(
      updateElementValueSelectCountyAgencyReducer({
        elementName: "status",
        value: "loading",
      })
    );
  }, [status == "success"]);
```

### Path: UserList.tsx

## Possible fix

I am not sure about the logic behind this code, but we shouldn't put conditions inside the dependency array of useEffect. If we want to run the useEffect only when the status is success, we should put the condition inside the useEffect.

```
useEffect(() => {
    if (status == "success") {
      dispatch(
        updateElementValueAgencyListReducer({
          elementName: "agencyList",
          value: agenciesForCounty,
        })
      );
      dispatch(
        updateElementValueSelectCountyAgencyReducer({
          elementName: "status",
          value: "loading",
        })
      );
    }
  }, [status]);
```
