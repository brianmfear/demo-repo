# SFDX Demo Push App

## Dev, Build and Test

```
    sfdx force:org:create ...
    sfdx force:source:push
    rm force-app/main/default/objects/Account/fields/Invalid_Field__c.field-meta.xml
    sfdx force:source:push
    sfdx force:org:open ...
```

## Issues

This simple repo is designed to show the force:source:push problem. If you follow the steps above precisely,
then view the Object Manager for this org, you'll see that Account.Valid_Field__c is not created in the org,
and cannot be pushed without modifying it or clearing the cache using:

```
    rm -r .sfdx/orgs/
    sfdx force:source:push
```

As a more complicated concept, imagine that there's a Visualforce page that failed to push because of
Account.Invalid_Field\_\_c not pushing; this means that you have to clear the cache to force the push to
happen. Using -f or -g will have no effect, because the system thinks the file has already been pushed,
and has not been modified.

The problem appears to be that `.sfdx/orgs/username/*.json` files are being updated for failed elements,
so they must be modified or the cache cleared in order to proceed. Clearing the cache with some odd
few thousand elements significantly delays the next push, as well, so this behavior is undesirable.
