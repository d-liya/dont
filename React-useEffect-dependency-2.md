# We shouldn't put variables that are changing in every render inside the dependency array of useEffect

```
let uTypeObj = [
  ROLE_COUNTY_ADMIN,
  ROLE_AGENCY_ADMIN,
  ...
];

 if (userType === ROLE_AGENCY_ADMIN) {
    uTypeObj = [ROLE_AGENCY_ADMIN, ROLE_POLICE_OFFICER];
  } else if (userType === ROLE_COUNTY_ADMIN) {
   ....
  }


  useEffect(() => {
    dispatch(
      listUser({
        userTypeObj: uTypeObj,
        status: "true",
      })
    );
    dispatch(getLoggedInUserCounties());
    dispatch(getLoggedInUserAgencies());
    isTokenValueAvailableForImport();
  }, []);

```

### Path to the bug: UserList.tsx

## Possible fix

Here we are using variables like uTypeObj that are changing in every render. We shouldn't put these variables inside the dependency array of useEffect. If we want to run the useEffect only when the value of uTypeObj changes, we should put the condition inside the useEffect and make sure to memoize the variable.

```
const uTypeObj = useMemo(() => {
  if (userType === ROLE_AGENCY_ADMIN) {
    return [ROLE_AGENCY_ADMIN, ROLE_POLICE_OFFICER];
  } else if (userType === ROLE_COUNTY_ADMIN) {
    return [ROLE_COUNTY_ADMIN, ROLE_AGENCY_ADMIN, ROLE_POLICE_OFFICER];
  } else if (userType === ROLE_STATE_ADMIN) {
    return [ROLE_STATE_ADMIN, ROLE_COUNTY_ADMIN, ROLE_AGENCY_ADMIN, ROLE_POLICE_OFFICER];
  } else {
    return [];
  }
}, [userType]);

useEffect(() => {
  dispatch(
    listUser({
      userTypeObj: uTypeObj,
      status: "true",
    })
  );
  dispatch(getLoggedInUserCounties());
  dispatch(getLoggedInUserAgencies());
  isTokenValueAvailableForImport();
}, [uTypeObj]);
```
