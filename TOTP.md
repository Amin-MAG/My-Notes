# TOTP

A time-based One-time Password (TOTP) is a computer algorithm that generates a one-time password (OTP) that uses the current time as a source of uniqueness.

TOTP is the cornerstone of the Initiative for Open Authentication (OATH), and is used in a number of two-factor authentication (2FA) systems.

## Algorithm

HMAC-based one-time password (HOTP) is a one-time password (OTP) algorithm based on hash-based message authentication codes (HMAC). It is a cornerstone of the Initiative for Open Authentication (OATH).

To establish TOTP authentication, the authenticator and authenticator must pre-establish both the [HOTP parameters](https://en.wikipedia.org/wiki/HMAC-based_One-time_Password_algorithm#parameters) and the following TOTP parameters:

Both the authenticator and the authenticated compute the TOTP value, then the authenticator checks whether the TOTP value supplied by the authenticated matches the locally generated TOTP value. Some authenticators allow values that should have been generated before or after the current time in order to account for slight [clock skews](https://en.wikipedia.org/wiki/Clock_skew), network latency, and user delays.

TOTP uses the HOTP algorithm, substituting the counter with a [non-decreasing](https://en.wikipedia.org/wiki/Increasing) value based on the current time:

TOTP value(K) = [HOTP value](https://en.wikipedia.org/wiki/HMAC-based_One-time_Password_Algorithm#Definition)(K, CT),

calculating counter value

${\displaystyle C_{T}=\left\lfloor {\frac {T-T_{0}}{T_{X}}}\right\rfloor ,}$

where

- $C_T$ is the count of the number of durations  between  and,
- $T$ is the current time in seconds since a particular [epoch](https://en.wikipedia.org/wiki/Epoch),
- $T_0$ is the epoch as specified in seconds since the [Unix epoch](https://en.wikipedia.org/wiki/Unix_epoch) (e.g. if using [Unix time](https://en.wikipedia.org/wiki/Unix_time), then  is 0),
- $T_X$ is the length of one-time duration (e.g. 30 seconds).

## Security

TOTP values can be [phished](https://en.wikipedia.org/wiki/Phishing) like [passwords](https://en.wikipedia.org/wiki/Password), though this requires attackers to proxy the credentials in real-time.

An attacker who steals the shared secret can generate new, valid TOTP values at will. This can be a particular problem if the attacker breaches a large authentication database.[4](https://en.wikipedia.org/wiki/Time-based_One-Time_Password#cite_note-4)

Because of latency, both network and human, and unsynchronised clocks, the one-time password must validate over a range of times between the authenticator and the authenticate. Here, time is downsampled into larger durations (e.g., 30 seconds) to allow for validity between the parties. 

### **Recovery Codes**

When a user loses access to their TOTP device, they would no longer have access to their account. Because TOTPs are often configured on mobile devices that can be lost, stolen, or damaged, this is a common problem. For this reason, many providers give their users "backup codes" or "recovery codes". These are a set of one-time use codes that can be used instead of the TOTP. These can simply be randomly generated strings that you store in your backend.

## Go Lang TOTP Package

To enroll a user, you must first generate an OTP for them.  Google Authenticator supports using a QR code as an enrollment method:

1. Generate a new TOTP Key for a User. `key,_ := totp.Generate(...)`.
2. Display the Key's Secret and QR-Code for the User. `key.Secret()` and `key.Image(...)`.
3. Test that the user can successfully use their TOTP. `totp.Validate(...)`.
4. Store TOTP Secret for the User in your backend. `key.Secret()`
5. Provide the user with "recovery codes". (See Recovery Codes bellow)

```go
import (
  "github.com/pquerna/otp/totp"

  "bytes"
  "image/png"
)

key, err := totp.Generate(totp.GenerateOpts{
     Issuer: "Example.com",
     AccountName: "alice@example.com",
})

// Convert TOTP key into a QR code encoded as a PNG image.
var buf bytes.Buffer
img, err := key.Image(200, 200)
png.Encode(&buf, img)

// display the QR code to the user.
display(buf.Bytes())

// Now Validate that the user's successfully added the passcode.
passcode := promptForPasscode()
valid := totp.Validate(passcode, key.Secret())

if valid {
	// User successfully used their TOTP, save it to your backend!
	storeSecret("alice@example.com", key.Secret())
}
```

Validating a TOTP passcode is very easy, just prompt the user for a passcode
and retrieve the associated user's previously stored secret.

1. Prompt and validate the User's password as normal.
2. If the user has TOTP enabled, prompt for the TOTP passcode.
3. Retrieve the User's TOTP Secret from your backend.
4. Validate the user's passcode. `totp.Validate(...)`

```go
import "github.com/pquerna/otp/totp"

passcode := promptForPasscode()
secret := getSecret("alice@example.com")

valid := totp.Validate(passcode, secret)

if valid {
// Success! continue login process.
}
```

## References

[Time-based One-Time Password - Wikipedia](https://en.wikipedia.org/wiki/Time-based_One-Time_Password)

[HMAC-based one-time password - Wikipedia](https://en.wikipedia.org/wiki/HMAC-based_one-time_password#parameters)

[GitHub - mdp/qrterminal: QR Codes in your terminal](https://github.com/mdp/qrterminal)

[GitHub - pquerna/otp: TOTP library for Go](https://github.com/pquerna/otp)