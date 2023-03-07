## MFA Challenge and Verify

This flow pack has two flows that will help you present an MFA challenge via push (Okta Verify) before a flow will continue. It relies on feeding in a target user email who will be the challenge recipient. The user must be enrolled in Okta Verify.

Since the flow is recursive, it is not possible to import the flow in its working state due to a missing reference catch 22. Instead, please import the flow as it is now and follow the instructions in the note in the "else" section of the if/elif card in UTIL - Poll MFA Status in order to complete the flow.
