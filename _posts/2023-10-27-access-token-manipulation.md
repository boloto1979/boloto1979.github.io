---
layout: post
title: Access Token Manipulation
description: "Is a cybersecurity technique with both legitimate and malicious applications. This method involves obtaining, modifying, or misusing access tokens used in authentication and authorization systems."
---

"Access Token Manipulation" refers to a cybersecurity technique used to obtain, modify, or abuse access tokens in authentication and authorization systems. Access tokens are used in authentication systems to confirm a user's identity and grant permissions to access resources or perform specific actions.

This technique can be used legitimately by applications and services to access resources on behalf of an authenticated user. However, in cybersecurity contexts, access token manipulation usually refers to malicious activities where an attacker attempts to exploit vulnerabilities in a system to obtain, modify, or misuse third-party access tokens.
<br>
<div style="text-align: center;">
  <img src="data:image/png;base64,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">
</div>
<br>
Some common forms of Access Token Manipulation include:

1. **Token Theft**: An attacker may try to steal access tokens from a legitimate user, such as intercepting them during transmission or exploiting security vulnerabilities.

2. **Token Reuse**: The attacker may attempt to reuse a valid access token after obtaining it, taking advantage of its validity to access resources or perform actions on behalf of the legitimate user.

3. **Token Modification**: This involves altering a valid access token to grant additional permissions or modify parameters to carry out malicious actions.

4. **Privilege Escalation**: An attacker may attempt to elevate their own privileges by manipulating access tokens to gain access to resources or functionalities they would not normally have permission for.

It's important to note that access token manipulation is a significant security threat to systems and applications that rely on authentication and authorization. To mitigate these threats, it's necessary to implement robust cybersecurity practices, such as properly securing tokens, monitoring suspicious activities, and adopting security measures like multi-factor authentication and encryption whenever possible.

## Legitimate Use Cases

While access token manipulation is often associated with malicious activities, there are legitimate use cases where applications use this technique. For example, in federated authentication systems, a service may manipulate an access token to access resources from another service on behalf of an authenticated user, as seen in Single Sign-On (SSO) with third-party services or social networks.

## OAuth 2.0 and Access Tokens

The OAuth 2.0 protocol is widely used to grant access to resources in third-party systems. It utilizes access tokens, often called "Bearer Tokens," to authorize requests. The security of access tokens is crucial for system integrity, and authentication providers should implement security measures, like token expiration, to reduce the risk of manipulation.

## Token Theft and Man-in-the-Middle Attacks

In token theft scenarios, attackers may use techniques like Man-in-the-Middle (MitM) attacks to intercept access tokens during communication between a client, an authorization server, and a resource server. This can occur in unsecured public networks or in untrustworthy mobile applications. To mitigate this, communications should be protected through encryption, and multi-factor authentication can be used to make token theft more challenging.

## Data Security Implications

Access token manipulation can have serious data security implications. If an access token is stolen and misused, a user's sensitive data can be compromised. Therefore, organizations must implement robust security practices, such as monitoring authentication events, logging suspicious activity, and token revocation policies.

## Privilege Elevation and Authorization Policies

Access token manipulation can also be used for privilege escalation, allowing an attacker to access resources or perform actions that would normally be prohibited. To mitigate this risk, systems should implement strict authorization policies that check whether a user has the appropriate permissions before granting access to resources.

In conclusion, Access Token Manipulation is a technique that can be used both legitimately and maliciously in authentication and authorization systems. Security is paramount to protect access tokens and ensure that only authorized users have access to sensitive resources and data. Implementing best security practices is essential to prevent and detect malicious activities related to token manipulation.
<br>
<div style="text-align: center;">
<img src="https://raw.githubusercontent.com/boloto1979/Basic-Types-of-Malware/main/Access%20Token%20Manipulation/images/accesstoken.png"/>
</div>
<br>
This [repository](https://github.com/boloto1979/Basic-Types-of-Malware/tree/main/Access%20Token%20Manipulation) contains two versions of the ATM (Assume The Identity of Another User) tool:

1. **ATM.cpp** - Windows version
2. **unix_ATM.cpp** - Linux version

Both versions aim to assume the identity of another user on the operating system where they are being executed, allowing a process to run with the privileges of that specific user.

## ATM.cpp - Windows Version

### Features:
- Assumes the identity of another user on the Windows system to execute a process with that user's privileges.
- Uses Windows API functions related to security and token management to gain access to a specific process and create a new process on behalf of the desired user.
- Allows specifying the PID (Process ID) of the process from which you want to assume the identity.

### Usage:
1. Compile the program using a C++ compiler that supports Windows libraries.
2. Run the program as an administrator or with elevated privileges to allow access to the required tokens and privileges.
3. Fill in the value of the `pid_to_impersonate` variable with the PID of the process you want to assume the identity of.
4. The program will request access to the token of the specified process, enable the SE_DEBUG_NAME privilege for debugging purposes, and then create a new process with the duplicated token on behalf of the desired user.

**Note:** Be cautious when using this program, as assuming the identity of other users without proper authorization may be a security violation.

## unix_ATM.cpp - Linux Version

### Features:
- Executes a process on the Linux system with superuser privileges (root) from the beginning.
- Allows specifying the PID of the process you want to assume the identity of.
- Creates a new process on behalf of the desired user.

### Usage:
1. Compile the program using a C++ compiler compatible with the Linux system.
2. Run the program with superuser privileges (root) to enable it to create processes with the necessary privileges.
3. Fill in the value of the `pid_to_impersonate` variable with the PID of the process whose identity you want to assume.
4. The program will create a new process with superuser privileges and the identity of the user associated with the specified PID.

**Note:** Running programs with superuser privileges can be dangerous and should be done with extreme caution, only for legitimate and authorized purposes.

## Security Warning:

Both versions of the tool are intended for educational purposes and should not be used for malicious purposes or without proper authorization. Manipulating user identities and privileges is a critical security issue and should be done responsibly and only on systems where you have permission to perform such actions. Misusing these tools may lead to legal consequences and significant harm.

**Use these tools with care and responsibility.**