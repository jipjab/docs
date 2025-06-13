# Share a user's OneDrive 

To allow another user to access a user's OneDrive as an admin (beyond just generating your own access link), you'll need to add them as a **Site Collection Administrator** on that OneDrive site. There are two effective methods to achieve this:

---

## 🎥 Quick video guide

[How to Give Access to OneDrive to Another User in Microsoft 365 Admin Center](https://www.youtube.com/watch?pp=0gcJCdgAo7VqN5tD&v=IEU_HoRv_CA&utm_source=chatgpt.com)

---

## 1. Use the Microsoft 365 Admin Center

1. Sign in to the **Microsoft 365 admin center** as a Global or SharePoint Admin.
2. Navigate to **Users → Active users**, and select the relevant user.
3. Open the **OneDrive** tab in their user properties.
4. Click **Create link to files** – this will generate access for you to their OneDrive ([learn.microsoft.com][1], [sharepointdiary.com][2]).
5. Use that link to access their OneDrive in your browser.
6. From there, click **Return to classic OneDrive**, then go to **Settings → Site settings → Site permissions** to assign another user (e.g., a manager or delegate) as a **Site Collection Administrator** ([learn.microsoft.com][1]).

---

## 2. Via SharePoint Admin Center

1. Go to the **SharePoint admin center** (via **Admin centers → SharePoint**).
2. Click **More features → User Profiles → Manage User Profiles**.
![alt text](/docs/img/SharePoint/CleanShot%202025-06-13%20at%2021.50.39.png)
![alt text](</docs/img/SharePoint/CleanShot 2025-06-13 at 21.53.48.png>)
3. Search for the user's profile.
![alt text](</docs/img/SharePoint/CleanShot 2025-06-13 at 22.04.52.png>)
4. Choose **Manage site collection owners**, and add the desired user as a **Site Collection Admin** ([reddit.com][3]).
5. Provide them with the OneDrive URL (e.g. `https://<tenant>-my.sharepoint.com/personal/username_domain_com`), and they can access it like their own OneDrive ([reddit.com][3]).

---