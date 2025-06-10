# Manual Path Analysis

WIth some techniques, you can analyze the path from your selected AD object to any your target.&#x20;

1. Type an account name or any AD object name in the search field at the top left corner. Hit Return.&#x20;

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

2. You will see the icon of the account name.

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

3. Right-click on the icon of the user account, and select 'Set as Starting Node'.&#x20;

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

4. Let's go to 'Node Info', scroll down the page, and select 'Transitive Object Control in OUTBOUND OBJECT CONTROL.&#x20;

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

If we are lucky, we have some good path to analyze. You will see something like, but I see one line to Domain icon with GenericAll privilege.

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

Let's analyze the further path by selecting Account Operators group and select 'Transitive Object Control' in OUTBOUND OBJECT CONTROL.&#x20;

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

This is a bit messy diagram to look at, but you can understand what the paths for your exploit and abuse are.&#x20;
