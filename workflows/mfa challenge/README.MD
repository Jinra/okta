## MFA Challenge and Verify

This flow pack has two flows that will help you present an MFA challenge via push (Okta Verify) before a flow will continue. It relies on feeding in a target user email who will be the challenge recipient. The user must be enrolled in Okta Verify.

Since the flow is recursive, it is not possible to import the flow in its working state due to a missing reference catch 22. Instead, please import the flow as it is now and follow the instructions in the note in the "else" section of the if/elif card in UTIL - Poll MFA Status in order to complete the flow. It should look like this once that's done.

<img width="1341" alt="Screenshot 2023-03-07 at 2 47 30 PM" src="https://user-images.githubusercontent.com/36873193/223571667-75c342e3-6c4b-4cf6-8b65-2f6b0f06cf77.png">
