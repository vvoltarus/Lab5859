# LEVEL 1 — SSH Service Recovery Incident

## Context

During LEVEL 1 network validation, the SSH service failed to start.

Initial configuration check:

<code>sudo sshd -t</code>
Error returned:

<code>no hostkeys available</code>
Further diagnostics revealed:
<code>error in libcrypto</code>

After partial correction, a new error appeared:

<code>missing privilege separation directory: /run/sshd </code>

Diagnostic Process
1. Configuration Validation
<code>sudo sshd -t</code>

Flag explanation:

<code>-t </code>— test mode. Validates configuration and required files without starting the daemon.

This is a safe method to verify SSH before restarting the service.

2. Host Key Inspection
<code>ls -l /etc/ssh/ssh_host_*</code>

Host keys were present but unusable due to cryptographic errors (libcrypto).

Root cause: corrupted SSH host key files.

Host keys are mandatory for SSH daemon identity and cryptographic negotiation.

Remediation Steps
1. Remove Corrupted Host Keys
<code>sudo rm /etc/ssh/ssh_host_*</code>

In a lab environment, regenerating host keys is acceptable.

Note: Host keys identify the server. Regenerating them will trigger a host verification warning on clients.

2. Regenerate Default Host Keys
<code>sudo ssh-keygen -A</code>

Flag explanation:

<code>-A</code> — generate all default host keys (RSA, ECDSA, ED25519) if missing.

3. Revalidate Configuration
<code>sudo sshd -t</code>

The host key error was resolved.

4. Privilege Separation Directory Error

New error:

<code>missing privilege separation directory: /run/sshd</code>

Cause:

<code>/run</code> is a temporary filesystem (tmpfs) recreated at boot.
The required runtime directory for SSH privilege separation was missing.

Privilege separation improves security by isolating unprivileged SSH processes.

5. Create Required Directory
<code>sudo mkdir -p /run/sshd
sudo chmod 0755 /run/sshd</code>

Flag explanation:

<code>-p</code> — create parent directories if needed.

0755 — owner: rwx; group: rx; others: rx.

6. Restart SSH Service
   
<code>sudo systemctl restart ssh
sudo systemctl status ssh </code>


Result: SSH service successfully started.

Lessons Learned

Always validate SSH configuration using sshd -t before restarting.

SSH host keys are mandatory for daemon startup.

Cryptographic errors often indicate corrupted key material.

<code>/run</code> is a volatile filesystem.

Correct diagnostic sequence prevents remote lockouts.
