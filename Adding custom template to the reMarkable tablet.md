# 📓 Add Custom Templates to reMarkable (Including Developer Mode Setup)

This guide will walk you through the steps to add your own **custom paper templates** (e.g., grid, golden ratio, lined) to your reMarkable tablet. You'll need **Developer Mode enabled**, access to your tablet via SSH, and basic command line familiarity.

---

## 🔓 1. Enter Developer Mode (Required)

> ⚠️ This will factory reset your tablet — **back up your files first!**

To enable Developer Mode:

1. Go to **Menu → Settings**.

2. Under **General > Software**, tap your **software version number**.

3. Tap **Advanced**.

4. Tap **Developer mode**.

5. Carefully read the information, then tap **Accept**.

6. Enter your passcode.

7. Tap **Continue** to start the factory reset.

Your tablet will **reboot into Developer Mode**, allowing SSH access.

---

## 📱 2. Connect to Your reMarkable via SSH

From your Mac (or Linux terminal):

```
ssh root@10.11.99.1
```

> If this doesn't work, make sure your tablet is connected to your computer via USB and your Mac is detecting it as a network device.

---

## 📦 3. Upload Your Custom Template

Create a folder named `my_templates` inside the template directory:

```
mkdir /usr/share/remarkable/templates/my_templates
```

From your **Mac terminal**, copy your PNG file (example: `golden_ratio_a4.png`) to the reMarkable:

```
scp golden_ratio_a4.png root@10.11.99.1:/usr/share/remarkable/templates/my_templates/
```

---

## 📝 4. Add Template Info to `templates.json`

Back in the SSH session, open the template list using the `vi` editor:

```
vi /usr/share/remarkable/templates/templates.json
```

Scroll to the end of the array and add your custom entry:

```
{  "name": "golden_ratio_a4",  "filename": "my_templates/golden_ratio_a4.png",  "iconCode": "template_grid",  "categories": ["custom"]}
```

> `name` must be **unique**, `iconCode` reuses built-in icons, and `filename` is your PNG’s path.

To save and exit in `vi`:

1. Press `ESC`

2. Type `:wq`

3. Hit `Enter`

---

## 🔓 5. Remount Filesystem as Writable

Before editing system files, ensure the root filesystem is writable:

```
mount -o remount,rw /
```

After you're done, you can remount it as read-only (optional):

```
mount -o remount,ro /
```

---

## 🔄 6. Reboot the reMarkable

To apply changes, reboot your tablet:

```
reboot
```

---

## ✅ 7. Done!

Your custom template should now appear under the **“custom”** category in the template browser, using the `` you specified.

---

## 🎨 Bonus: Custom Icons?

Currently, the icon used in the template list comes from a **predefined set of system icons**. You can set `iconCode` to reuse an existing one (like `template_grid`, `template_music`, `template_weekplanner`), but **you can't add new icon images without deep system hacks**.

---

## 🧠 Tip: SCP vs SSH

- Use `` from your **Mac** to copy files:
  
  ```
  scp file.png root@10.11.99.1:/target/path/
  ```

- Use `` to access the reMarkable’s internal shell and edit files:
  
  ```
  ssh root@10.11.99.1
  ```
