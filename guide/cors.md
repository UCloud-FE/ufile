

# Cross-domain access

Cross-domain access (CORS) is an operation when a user requests resources from a web page of one domain to another domain. To address this issue, US3 provides cross-domain access policy management features that make it easy for you to restrict the permissions for cross-domain access.

When US3 receives a cross-domain request, it will read the list of cross-domain access policies configured in the storage space, and match each policy one by one according to its Origin, and When US3 receives a cross-domain request, it will read the list of cross-domain access policies configured in the storage space, and match each policy one by one according to its Origin, and the first eligible policy information will be added to the CORS-related Header and returned to the user.

## Set cross-domain access policy

Select the corresponding space and choose the Cross Domain Settings button in the right action.

! [](/images/guide/cross-domain settings 1-1.png)

Click the New Policy button to bring up the New Cross-Domain Access Policy screen.

! [](/images/cross-domain-settings2.png)

1. Source (Origin): user can fill in the source of allowed cross-domain requests, you can set more than one, one per line, and up to one wildcard character (*) per line. ) per line.

AllowMethods: user can choose the allowed cross-domain request methods.

Allow-Headers: users can fill in the allowed cross-domain request Header, you can set multiple, one per line, up to one wildcard (*) per line.

Expose-Headers: user can fill in the response headers that are allowed to be accessed from the application, multiple can be set, one per line, no The user can fill in the response headers that are allowed to be accessed from the application, multiple can be set, one per line, no wildcard (*) is allowed.

The cross-domain access policy takes effect by default after it is added, and you can edit and delete it accordingly by using the other buttons under the The cross-domain access policy takes effect by default after it is added, and you can edit and delete it accordingly by using the other buttons under the action bar.

! [](/images/cross-domain settings3.png)

## Remarks

Configure up to 10 cross-domain access policies per storage.
