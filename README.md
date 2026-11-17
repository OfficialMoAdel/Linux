# 🐧 Linux & DevOps Study Notes

A personal knowledge base of **Obsidian Flavored Markdown** notes covering Linux fundamentals, system administration, shell scripting, Apache httpd, SELinux, and L3 production troubleshooting.

> [!NOTE] Best viewed in Obsidian
> These notes use [Obsidian Flavored Markdown](https://help.obsidian.md/obsidian-flavored-markdown) (wikilinks, callouts, properties, embeds). They resolve links natively in [Obsidian](https://obsidian.md/). On GitHub, wikilinks (`[[Note]]`) appear as plain text — open the linked files below to read each note.

## 📚 Notes

| # | Note | What it covers | Tags |
|---|------|----------------|------|
| 1 | [Linux Fundamentals](Linux%20Fundamentals.md) | CLI, architecture, file system, navigation, users & groups, permissions, systemd, networking, package management, troubleshooting | `#linux` `#cli` `#devops` `#study-notes` |
| 2 | [Linux System Administration](Linux%20System%20Administration.md) | Task scheduling (`at`/`cron`/`anacron`/systemd timers), logrotate, ACLs, networking, FreeIPA, storage, monitoring | `#sysadmin` `#rhel` `#rhcsa` `#networking` |
| 3 | [Linux Shell Scripting - Complete Guide](Linux%20Shell%20Scripting%20-%20Complete%20Guide.md) | Variables, conditionals, loops, functions, arrays, strings, error handling, logging, backup, monitoring, interview prep | `#shell-scripting` `#automation` `#devops` `#interview-prep` |
| 4 | [SELinux — دليل عملي](SELinux.md) | Security contexts, modes (enforcing/permissive/disable), labels (`chcon`/`semanage`/`restorecon`), booleans, port changes, denial logs | `#selinux` `#security` `#rhel` |
| 5 | [Apache (httpd) — Virtual Hosts & SSL Complete Lab Guide](Apache%20httpd.md) | Install httpd, name-based virtual hosts, self-signed SSL/HTTPS, firewall (RHEL 9 lab with real terminal output) | `#apache` `#httpd` `#ssl` `#web-server` `#rhel9` |
| 6 | [Linux L3 Troubleshooting Notes](Linux%20L3%20Troubleshooting%20Notes.md) | RHEL upgrades, boot/initramfs, SELinux, TLS/certificates, emergency shell, Ansible, CI/CD secrets, post-quantum crypto | `#troubleshooting` `#l3-support` `#rhel` `#tls` |

## 🗺️ How to use

### In Obsidian (recommended)

1. Clone this repo:
   ```bash
   git clone <your-repo-url>
   cd Linux
   ```
2. Open Obsidian → **Open folder as vault** → select the cloned folder.
3. Wikilinks (`[[Note Name]]`), embeds (`![[...]]`), callouts, and frontmatter properties resolve automatically.

### On GitHub

- Click any note name in the table above to view it on GitHub.
- Wikilinks and embeds are Obsidian-specific and do not render as links on GitHub; navigate via the file list instead.

## 🏷️ Tags used

`#linux` `#sysadmin` `#devops` `#shell-scripting` `#selinux` `#security` `#apache` `#httpd` `#ssl` `#troubleshooting` `#rhel` `#rhcsa` `#l3-support` `#tls` `#automation` `#interview-prep` `#study-notes`

## 🤝 Contributing

This is a personal study repository. Issues and pull requests with corrections or additions are welcome.

## 🔗 Related

- [Obsidian Flavored Markdown](https://help.obsidian.md/obsidian-flavored-markdown)
- [Obsidian Help — Internal links](https://help.obsidian.md/links)
- [Obsidian Help — Callouts](https://help.obsidian.md/callouts)
- [Obsidian Help — Properties](https://help.obsidian.md/properties)
