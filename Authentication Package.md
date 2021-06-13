# Authentication Package

Authentication includes:

- Sign up
- Sign in
- Email Verification
- SMS Verification
- 2 Factor Authentication
- IP Tracking for sessions

# Entities

![Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled.png](Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled.png)

## Users

- username: It should be unique.
- email: It could be a requirement and could be unique. (based on the package configuration)
- phoneNumber: It could be a requirement and could be unique. (based on the package configuration)
- is2FAEnable: 2FA could be optional or be a requirement. For the case that it is an optional feature, we need to have this field for each user.
- isPhoneVerified: It could be essential for registration or 2FA. If we can distinguish the users who verified their phones, it is possible to have different kinds of behavior with them.
- isEmailVerified: It could be essential for registration or 2FA. If we can distinguish the users who verified their emails, it is possible to have different kinds of behavior with them.

## Verification Request

Verification request is responsible for managing phone, email verifications. It might be for 2FA, Registration, or just a verification that the user owes that email or phone.

![Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%201.png](Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%201.png)

- type: It is VIA_SMS or VIA_EMAIL.
- token: It is the token that verification can be confirmed with.
- isVerified: It shows if this request has been completed successfully or not.
- expirationTime: It shows the expiration date and isVerified will not be updated after this time.

## Login History

This entity is responsible for our Login requests.

![Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%202.png](Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%202.png)

## Access Requests

These kinds of entities are responsible for giving special access to the user by verification.

### Password Recovery Requests

The “RecoveryRequest” entity has a one-to-one relation with “VerificationRequest”. So we can find out whether a recovery request has a successful verification or not. If someone wants to click on resend button in this scenario we should create a new verification request again and attach it to the recovery request row (by a foreign key). Each recovery request itself has an expiration date. It means even if you verify your email address to recover your password, this access will be expired in due time. Once you use this accessibility or this token, It should not available anymore. (Because each access permission is just for a single time)

![Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%203.png](Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%203.png)

- token: It is the key that we can access special permission with.
- isEnable: At first it should be true and after using this token we should disable it.
- expirationDate: It shows the expiration date of this token.

### Multi-FA Requests

If 2FA is enabled for a user, each time the user logs in with a different IP address, he or she should verify the email or phone number. A one-to-one relation between “MultiFactorAuthRequests” and “LoginHistory” shows the valid sessions.

> ⚠  But we should handle some cases:What if a user change his or her password?What if a user terminate some sesssions that he or she used before?

Just like the password recovery the access token has an expiration time. Even if you verify your email or phone, but you do not use the token to give full access, it will be expired at a due time. It is also disposable. If everything is ok, then the user will receive access and refresh token.

![Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%204.png](Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%204.png)

- token: It is the key that we can access special permission with.
- isEnable: At first it should be true and after using this token we should disable it.
- expirationDate: It shows the expiration date of this token.

> ⚠  As They both have the same kinds of attributes. We can think about merging these entities. The name might be something like Access Requests.

## Sessions

We can show the user sessions later. We can also change our behavior when a user logs in from the same IP address. (For example, not asking for 2FA). Each user may have many sessions.

![Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%205.png](Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%205.png)

# The last version of ER

![Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%206.png](Authentication%20Package%206dfecbf48c99490bba810b83ca58d811/Untitled%206.png)