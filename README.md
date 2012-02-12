Overview
========

`ssh-find-id` searches for keys in the authorized\_keys file on remote hosts
that match a regular expression or a key from the SSH agent or an identity
file.

Usage
=====

<code>ssh-find-id [<em>PATTERNS</em>] [<em>OPTIONS</em>] [--] [<em>USER</em>@]<em>MACHINE</em> ...</code>

If no *PATTERNS* are specified, by default `ssh-find-id` will search for the
keys represented by the SSH agent and the keys in ~/.ssh.identity.pub,
~/.ssh/id\_rsa.pub and ~/.id\_dsa.pub.

Patterns
--------
<dl>
<dt><code>-L</code></dt>
<dd>Search for all public keys represented by the SSH agent.</dd>
<dt><code>-i <em>FILE</em></code></dt>
<dd>Search for the public key in <em>FILE</em>. The <code>-i</code>
    argument can be provided multiple times to search for multiple keys.
</dd>
<dt><code>-e <em>PATTERN</em></code></dt>
<dd>Search for public keys that match the regular expression
    <em>PATTERN</em>. The <code>-e</code> argument can be provided multiple
    times to search for multiple keys.
</dd>
</dl>

Options
-------
<dl>
<dt><code>-o <em>OPTION</em></code></dt>
<dd>Specify additional SSH options. See
    <a href="http://linux.die.net/man/5/ssh_config">ssh_config(5)</a> for
    the options and their possible values. The <code>-o</code> argument can
    be provided multiple times.
</dd>
<dt><code>--color <em>WHEN</em></code></dt>
<dd>Colorize output. <em>WHEN</em> may be <code>never</code>,
    <code>always</code> or <code>auto</code>, and defaults to
    <code>auto</code>.
</dd>
<dt><code>--</code></dt>
<dd>Treat all remaining arguments as host names.</dd>
<dt><code>--help</code></dt>
<dd>Display a usage message.</dd>
</dl>

Examples
========

Check example.com:~/.ssh/authorized\_keys for the keys represented by the SSH
agent and in ~/.ssh/identity.pub, ~/.ssh/id\_rsa.pub and ~/.ssh/id\_dsa.pub.

```console
user@laptop:~$ ssh-find-id example.com
example.com:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com
user@laptop:~$
```

Check example.com for keys that contain `@laptop` or `@desktop`.

```console
user@laptop:~$ ssh-find-id -e @laptop -e @desktop example.com
example.com:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com
user@laptop:~$
```

Use bash's brace expansion to search for keys for user and root at
www.example.com and its four backup servers, and use only public key
authentication so you aren't prompted for a password for servers that don't
have your public key.

```console
user@laptop:~$ ssh-find-id -e @laptop -o PreferredAuthentications=publickey {root@,}www{,{2..5}}.example.com
root@www.example.com:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com
root@www2.example.com:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com
Permission denied (publickey,gssapi-with-mic,password).
Permission denied (publickey,gssapi-with-mic,password).
Permission denied (publickey,gssapi-with-mic,password).
www.example.com:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com
www2.example.com:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com
www3.example.com:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com
www4.example.com:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC35pzENEY8dhsFTnl0t4pyXsokhQArdYa1eZ5BBA97f6pt/X4wo/Hgcsf0GAtFUgwRcNxy5Ca7zpDkVacSr3y4vIYgZt8bdv1mSHDQSmBqy/cMNk8042hzusq5EL71GMRS+H6ZW/Lv40JXSH0Ah3nrL2CuvFZVmn9bSw28ZiQKaDizLGzw0lotV2xKE0Bw8hB9m8MfiRalSn1xlwbtwUz5HcW+1K3kaLpjXbhNQk+gHaAtF8YVilXEnNmUqiUqXubUGZfDK47QU35HEVKs5lkm86d2dw4wKHejHZO+aa/7bUxFt/0/OurEt4LsrMTmhwwlrSmp9vaXhG7S8EMIaKil user@example.com
Permission denied (publickey,gssapi-with-mic,password).
user@laptop:~$
```
