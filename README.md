Overview
========

`ssh-find-id` searches for keys in the authorized\_keys file on remote hosts
that match a regular expression or a key from the SSH agent or an identity
file.

Usage
=====

`ssh-find-id` [*PATTERNS*] [*OPTIONS*] [--] [*USER*`@`]*MACHINE* ...

If no *PATTERNS* are speified, by default `ssh-find-id` will search for the
keys represented by the SSH agent and the keys in ~/.ssh.identity.pub,
~/.ssh/id\_rsa.pub and ~/.id\_dsa.pub.

Patterns
--------
<table>
<tr><th><code>-L</code></th>
    <td>Search for all public keys represented by the SSH agent.</td>
</tr>
<tr><th><code>-i</code> <em>FILE</em></th>
    <td>Search for the public key in <em>FILE</em>. The <code>-i</code>
        argument can be provided multiple times to search for multiple keys.
    </td>
</tr>
<tr><th><code>-e</code> <em>PATTERN</em></th>
    <td>Search for public keys that match the regular expression
        <em>PATTERN</em>. The <code>-e</code> argument can be provided multiple
        times to search for multiple keys.
    </td>
</tr>
</table>

Options
-------
<table>
<tr><th><code>-o</code> <em>OPTION</em></th>
    <td>Specify additional SSH options. See
        <a href="http://linux.die.net/man/5/ssh_config">ssh_config(5)</a> for
        the options and their possible values. The <code>-o</code> argument can
        be provided multiple times.
    </td>
</tr>
<tr><th><code>--color</code> <em>WHEN</em></th>
    <td>Colorize output. <em>WHEN</em> may be <code>never</code>,
        <code>always</code> or <code>auto</code>, and defaults to
        <code>auto</code>.
    </td>
</tr>
<tr><th><code>--</code></th>
    <td>Treat all remaining arguments as host names.</td>
</tr>
<tr><th><code>--help</code></th>
    <td>Display a usage message.</td>
</tr>
</table>

Examples
========

Check example.com:~/.ssh/authorized\_keys for the keys represented by the SSH
agent and in ~/.ssh/identity.pub, ~/.ssh/id\_rsa.pub and ~/.ssh/id\_dsa.pub.

<pre><code>
<span style="color:#00DD00">user@laptop</span>:<span style="color:#0000DD">~</span>$ ssh-find-id example.com
<span style="color:#DDAA00">example.com</span>:<span style="color:#DD0000">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com</span>
<span style="color:#00DD00">user@laptop</span>:<span style="color:#0000DD">~</span>$
</code></pre>

Check example.com for keys that contain `@laptop` or `@desktop`.

<pre><code>
<span style="color:#00DD00">user@laptop</span>:<span style="color:#0000DD">~</span>$ ssh-find-id -e @laptop -e @desktop example.com
<span style="color:#DDAA00">example.com</span>:<span style="color:#DD0000">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com</span>
<span style="color:#00DD00">user@laptop</span>:<span style="color:#0000DD">~</span>$
</code></pre>

Use bash's brace expansion to search for keys for user and root at
www.example.com and its four backup servers, and use only public key
authentication so you aren't prompted for a password for servers that don't
have your public key.

<pre><code>
<span style="color:#00DD00">user@laptop</span>:<span style="color:#0000DD">~</span>$ ssh-find-id -e @laptop -o PreferredAuthentications=publickey {root@,}www{,{2..5}}.example.com
<span style="color:#DDAA00">root@www.example.com</span>:<span style="color:#DD0000">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com</span>
<span style="color:#DDAA00">root@www2.example.com</span>:<span style="color:#DD0000">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com</span>
Permission denied (publickey,gssapi-with-mic,password).
Permission denied (publickey,gssapi-with-mic,password).
Permission denied (publickey,gssapi-with-mic,password).
<span style="color:#DDAA00">www.example.com</span>:<span style="color:#DD0000">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com</span>
<span style="color:#DDAA00">www2.example.com</span>:<span style="color:#DD0000">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com</span>
<span style="color:#DDAA00">www3.example.com</span>:<span style="color:#DD0000">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com</span>
<span style="color:#DDAA00">www4.example.com</span>:<span style="color:#DD0000">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com</span>
Permission denied (publickey,gssapi-with-mic,password).
<span style="color:#00DD00">user@laptop</span>:<span style="color:#0000DD">~</span>$
</code></pre>
