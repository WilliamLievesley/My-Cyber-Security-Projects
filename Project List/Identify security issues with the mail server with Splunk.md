# Identify Security Issues with the Mail Server Using Splunk
[Full Document](https://docs.google.com/document/d/1BVIJW8u04saWFWxRLXt_Iq9QmJgPdPsqx9nuQSbfWfI/edit?tab=t.0#heading=h.showwcjcgtjz)
## Scenario
You are a security analyst working at the e-commerce store **Buttercup Games**. You've been tasked with identifying whether there are any possible security issues with the **mail server**. To do so, you must explore any failed **SSH logins** for the root account.

---

## Inputting the Mail Server Data into Splunk

To add the data, I navigated to **Settings** and clicked on the **Add Data** icon, inputting the file containing mail server logs.

![Placeholder for Add Data Screenshot](https://github.com/WilliamLievesley/My-Cyber-Security-Projects/blob/main/Project%20List/images/Identify%20Security%20Issues%20with%20the%20Mail%20Server%20Using%20Splunk/add%20data.png)

---

## Verifying Data Ingestion and Indexing in Splunk: First Query Execution

To confirm that the data was successfully ingested, I performed a basic search query:

```splunk
index="main"
```

The `main` index is the default location where data is stored. Upon executing the query, log events were displayed, confirming that the data was successfully indexed. To ensure a comprehensive search, I set the **time range** to **"All Time"**.

![Placeholder for Query Result Screenshot](https://github.com/WilliamLievesley/My-Cyber-Security-Projects/blob/main/Project%20List/images/Identify%20Security%20Issues%20with%20the%20Mail%20Server%20Using%20Splunk/index%20main%20host.png)

---

## Searching Through the Mail Server Logs

Since I have been tasked with investigating **failed SSH logins** for the root account on the mail server, I refined my search by specifying the host:

```splunk
index="main" host=mailsv
```

(`mailsv` represents the **mail server**)

![Placeholder for Mail Server Search Screenshot](https://github.com/WilliamLievesley/My-Cyber-Security-Projects/blob/main/Project%20List/images/Identify%20Security%20Issues%20with%20the%20Mail%20Server%20Using%20Splunk/index%20main%20host.png)

---

## Finding the Failed Login Attempts for Root

To search specifically for failed login attempts, I performed the following query:

```splunk
index="main" host=mailsv fail* root
```

This query returned **346 events**, revealing multiple failed **SSH login attempts** for the root user.

### **What This Data Indicates**
- These failed login attempts suggest a possible **brute force attack** targeting SSH.
- The **timestamps** indicate that the attacks occurred in rapid succession, implying the use of an **automated attack script** or a **bot**.
- The logs contain **various IP addresses**, which may indicate the use of **proxies, VPNs, or a botnet** to evade detection.

![Placeholder for Failed Login Screenshot](https://github.com/WilliamLievesley/My-Cyber-Security-Projects/blob/main/Project%20List/images/Identify%20Security%20Issues%20with%20the%20Mail%20Server%20Using%20Splunk/index%20main%20host%20root.png)

---

## Conclusion
This behavior aligns with a **brute-force attack** on the SSH service of the mail server. The attacker is attempting to guess the **root password** to gain unauthorized access. The repetitive login attempts in a short time span suggest the use of a **botnet**.

### **Potential Next Steps**
- **Implement rate limiting** to reduce brute-force attempts.
- **Deploy fail2ban** or similar tools to block repeated failed login attempts.
- **Restrict SSH access** to only authorized IP addresses.
- **Enforce key-based authentication** instead of password-based authentication.

---

By leveraging Splunk's search capabilities, I was able to detect suspicious activity on the mail server, which can now be mitigated using security best practices.

