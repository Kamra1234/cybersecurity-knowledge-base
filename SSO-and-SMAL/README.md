What is SSO (Single Sign-On)?
By providing access to multiple applications through a single set of credentials (e.g. password, security token, digital certificate), it simplifies user authentication and significantly improves the user experience (UX).
Implementing SSO solutions increases security by reducing the attack surface, reducing password fatigue, and allowing IT teams to centrally manage user permissions.
Using SSO, organizations can enforce stronger security policies such as MFA (Multi-Factor Authentication) across all related services to prevent unauthorized access and data breaches.
A simple example
means that a user logs in only once and then accesses multiple services/applications without having to enter a password each time.
When you log in with your Google account, you are automatically granted access to Google Drive and Gmail without having to enter your password again.
Note: SSO is a process. The protocol that implements it is called SAML.

Analogous Example
Imagine customers who have already entered a bar are asked to show their ID to prove their age every time they try to purchase an alcoholic beverage. Some customers quickly become frustrated with these constant checks and may even try to bypass the process.
However, most places only verify a customer’s identity once and serve multiple drinks to the customer throughout the night.
This is somewhat like an SSO system: instead of the user having to prove their identity multiple times, they authenticate once and can then access multiple different services.

What are the benefits of SSO?
In addition to the convenience and simplicity for users, SSO is also widely considered to be a more secure solution.
This may seem a bit of a paradox: How can entering a password once be more secure than logging in multiple times with multiple different passwords?
Advocates of SSO cite the following reasons:
1. Stronger passwords
Since a user only uses one password, it is easier for them to remember and use stronger passwords.
2. No password duplication
When a user has to remember passwords for multiple applications and services, a condition called password fatigue is likely to develop. Using the same password across multiple services is a major security risk.

Credential Stuffing
We have a type of attack for this situation called Credential Stuffing:
A program/application or anything else is hacked and the hacker gains access to the database and all the user information, including passwords, usernames and many more, is leaked for whatever reason (or it is not leaked at all and the same hacker or another hacker does it).
He also tests that password inside other programs/applications, because most people use the same passwords for their different programs and this is a security bug.
Note: This is different from a Brute Force attack.

Weaknesses of SSO
It can also be considered that although SSO can increase security, it still has its own weaknesses.
If for any reason your password is leaked in SSO and falls into the hands of a hacker, for example through Phishing or Social Engineering, then the hacker can log in to all services/applications and gain full access.
This is exactly what happens with SSO: instead of having to hack 10 services individually, the hacker only needs to target one point (the Identity Provider).

1. Golden SAML — The most dangerous case
This is similar to a more famous attack in the Windows/Active Directory world called Golden Ticket.
In SAML, when the IdP creates a token, it signs it with a private key to ensure that it is not fake.
Now if the hacker can steal that same signing key, he is no longer limited to stealing just one token; he can create fake tokens for every user and every service and sign them with that same private key. That is, the hacker effectively becomes the IdP himself.

2. IdP as the main target
What does this mean?
First, we need to see what an IdP is.
An IdP (Identity Provider) is a service that stores and verifies the user's identity; that is, a system that is responsible for verifying the user's identity. This system checks whether the password/username that the user enters is correct or not and tells the rest of the system (SP - Service Provider) that this is the real user and that you should trust him.
Example
When you want to log in to a company's Slack,
Slack (which is the SP here) says, "Go log in with Okta (which is the IdP).
You are redirected to company.okta.com.
There you enter your username/password and possibly MFA.
Okta creates a signed token that says: "This user is verified."
That token goes back to Slack, and Slack then lets you in.
Now back to the main topic. Why would an IdP be a prime target for hackers?
Imagine you have a company with 20 different systems (email, database, admin panel, file server, etc.).
Without SSO, a hacker would have to hack each one individually; it’s a pain.
But when everyone uses an IdP (like Okta or Azure AD), the hacker doesn’t have to look for 20 targets, and their target is clear. They just go for that one IdP. If they can get it, they’ve got the company, because the entire company and systems trust the IdP and Okta.
In Red Teams, the first thing they focus on is the IdP, because it has the most value for attack.

3. The token is stolen, no password required
Important point:
In SSO, after you log in once, the system does not ask you for a password every time. Instead, it gives you a token that shows: “This is the same user whose identity has been previously verified.”
Now, if a hacker can steal this token (for example, through an XSS security hole or traffic interception), he does not need your password at all, because he has the token itself. It is as if he has stolen the entrance card and does not need to forge your identity anymore.

In summary
SSO provides a lot of convenience and in one sense can also increase security, but on the other hand, because instead of 20 doors, it uses one door for all services, if this one door is broken, the whole house opens.
The hacker always looks at the IdP as the most valuable target.

Methods of defense against these attacks
1. Golden SAML
Question: What is being stolen?
The private key of the IdP signature.
Question: How can we protect it?
If this key is stored inside a Hardware Security Module (HSM) instead of normally stored on the server disk, it becomes much harder to steal.
The key can also be changed periodically so that even if a copy of it is leaked, it can no longer be used.

2. IdP as the main target
Question: What is being stolen?
Full access to the IdP (admin account, admin panel).
Question: How can we protect it?
Since this is the most valuable system, it should have the strictest security rules.
• MFA for each IdP admin
• Restrict admin panel access to specific IPs only (not from anywhere)
• Define rules that allow login only during business hours and from a specific domain (e.g. company).
• Keep admin accounts separate from everyday accounts
• Use principles like MAC and RBAC to increase security
• Use standards like OATH

3. Token is stolen
Question: What is stolen?
The token itself (issued after login).
Question: How can we protect it?
If the token has a short lifespan, e.g. it expires after 15 minutes, even if it is stolen, it will be useless.

Sources: Cloudflare