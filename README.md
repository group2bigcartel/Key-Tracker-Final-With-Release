# Room Key Tracker

A Windows desktop app that tracks who has borrowed which room key. People identify themselves with a QR-coded ID and a personal password, then scan the key's QR code to borrow or return it. Built for **Isabela State University** by **H-E-I-M-A (Group 2)**, 2026–2027.

![.NET 8](https://img.shields.io/badge/.NET-8.0-512BD4)
![VB.NET](https://img.shields.io/badge/language-VB.NET-blue)
![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11-lightgrey)

## Features

**Borrowing and returning**
- Three-step flow: scan your ID QR, enter your password, scan the room key QR.
- Confirm screen before anything is saved. Borrow if the room is vacant, return if you hold the key, blocked if someone else has it.
- Webcam scanning with automatic camera selection (virtual and IR cameras are skipped).
- Side panels on the Scan tab: a how-to guide that highlights the current step, the keys currently borrowed, and the 5 most recent log entries.

**Rooms board**
- Live vacant and occupied counts.
- Rooms grouped by floor, highest floor on top and floor 1 at the bottom. Rooms that are not plain numbers (for example `LAB-A`) go in an "Other rooms" group.
- Refreshes automatically.

**Log**
- Records every borrow, return, person removed, room removed and password reset, with date and time.
- Keeps the latest 200 entries.

**Admin (password protected)**
- **Key QRs:** one QR per room, sorted by room number and grouped by floor. Add or remove rooms and print all QR codes.
- **Register:**
  - *Generate ID* creates a new user and asks for their password.
  - *Use existing QR* links a person to a QR they already own, then asks for their password.
  - *Show QR* displays a generated QR, and needs that person's own password.
  - *Reset password* sets a new password for a user.
  - *Remove selected* deletes a user (the key must be returned first).
- **Log & Password:** clear the log and change the admin password.
- The admin password is created the first time the Admin tab is opened. The Admin tab stays hidden until the password is correct.

**Other**
- Full-screen kiosk mode. Press **F11** to toggle.
- School seal as the window and .exe icon, and a "Meet the team" dialog.

## How it works

| Step | Action |
|------|--------|
| 1 | Scan your **ID QR** (faculty or OJT). |
| 2 | Enter **your password**. You get 3 tries. |
| 3 | Scan the **room key QR**. |
| 4 | Confirm **borrow** or **return**. |

QR contents:

| QR | Text inside |
|----|-------------|
| User ID | `USER:<id>`, for example `USER:FAC-001` or `USER:OJT-002` |
| Room key | `ROOM:<room>`, for example `ROOM:101` |

User IDs are `FAC-###` for faculty and `OJT-###` for OJT trainees.

## Requirements

- Windows 10 or 11
- [.NET 8 SDK](https://dotnet.microsoft.com/download) or Visual Studio 2022
- A webcam
- Internet on the first build, to restore NuGet packages

## Build and run

**Visual Studio:** open `RoomKeyTracker.sln` and press **F5**.

**Command line:**

```
dotnet run
```

**Publish a release build:**

```
dotnet publish -c Release -r win-x64 --self-contained false
```

The first launch has no admin password. Open the **Admin** tab and the app asks you to create one.

### NuGet packages

| Package | Version | Used for |
|---------|---------|----------|
| QRCoder | 1.8.1 | Generating QR codes |
| ZXing.Net | 0.16.11 | Reading QR codes |
| ZXing.Net.Bindings.Windows.Compatibility | 0.16.16 | ZXing with System.Drawing |
| AForge.Video.DirectShow | 2.2.5 | Webcam capture |

A `NU1701` warning about AForge is expected and is suppressed in the project.

## Data and security

- Data is saved to `%AppData%\RoomKeyTracker\data.json`. Delete this file to reset the app.
- Admin and user passwords are never stored in plain text. Each one is saved as a salted PBKDF2-SHA256 hash (100,000 iterations).
- `data.json` itself is not encrypted, so it contains names, IDs and the log in readable form. Keep the computer and that folder protected.
- Users registered before passwords were added are asked to create one the first time they scan.
- A forgotten user password can be replaced by the admin with **Reset password**. Passwords cannot be recovered.

## Project structure

```
RoomKeyTracker/
├── RoomKeyTracker.sln
├── RoomKeyTracker.vbproj    Project, NuGet packages, app icon and file details
├── ISU_logo.ico             Application icon
├── Program.vb               Data classes, JSON storage, password hashing, entry point
├── MainForm.vb              UI and app logic
├── MainForm.Designer.vb     WinForms designer file
├── MainForm.resx
└── BrandAssets.vb           School seal and H-E-I-M-A logo, embedded as Base64
```

## Team

**H-E-I-M-A, Group 2**

| Name | Role |
|------|------|
| Trinidad, Andrae | Project Manager |
| Somera, Marc | UI Design |
| Clemente, Hasley Jay | Backend |
| Gidden, Eryl | Backend |
| Nuez, Caezar Ian | Debugger / Tester |

## License

© 2026–2027 H-E-I-M-A. All rights reserved. If you want others to be able to reuse the code, add a `LICENSE` file (for example MIT) to this repository.
