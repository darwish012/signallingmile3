# Signalling, Milestone 3: Shop Ordering Apps and Voice/Video Calling (Android)

Milestone 3 of the signalling project: a set of Android applications written in Kotlin. The main part is a two-app shop ordering system (a buyer app and a seller app) that shares a Firebase backend. The milestone also contains three calling and messaging experiments: a text chat with voice recording over TCP, a video/audio call app using ZEGOCLOUD, and an RTP voice call app. All projects are stored as zip archives.

## Contents

| File | Description |
| --- | --- |
| `milestone_3.zip` | Archive containing the five items listed below. |
| `voipfinal.zip.zip` | Separate Android Studio project `voipfinal` (RTP voice call). The name is a zip inside a zip file name; unzip once to get the project. |

Inside `milestone_3.zip`:

| Item | Description |
| --- | --- |
| `buyertobesubmitted.zip` | Buyer app (`buyer` project, package `com.example.signaling`). |
| `sellertobesubmited.zip` | Seller app (`seller` project, package `com.example.signaling`). |
| `savedsendtextandplayaudio2.zip` | Project `voip3`: text messages over TCP plus voice recording and playback. |
| `yarab.zip` | Project `yarab`: video and audio calls with ZEGOCLOUD. |
| `mile3photos.rar` | A RAR archive; not inspected when this README was written. |

The buyer and seller projects also contain further nested zip files (for example `buyercartnolimit.zip`, `buyer1.zip`, `prodtocart.zip`, `address.zip`); these were not inspected.

## Shop ordering system (buyer and seller apps)

Both apps use Firebase Authentication (email and password), the Firebase Realtime Database and Firebase Storage (product, shop and profile pictures).

**Buyer app**
- Sign up and log in; the buyer record (name, email, picture) is stored under `buyers/<uid>`.
- A Shops tab lists all registered shops (from `sellers info`); a Profile tab lets the buyer edit the name and picture.
- Opening a shop shows its products (from `sellers/<shop uid>`) with a cart tab. Product quantities are kept in `buyers/<uid>/cart`.
- The cart shows the price before taxes, a 5% tax, and the total including taxes, with delete-cart and order-now buttons.
- At checkout the buyer types an address or chooses to pick the order up at the shop; the order is written to `sellers orders/<shop uid>/<buyer uid>`.
- The order screen listens to the order status and shows: cancelled (-1), accepted and being prepared (1), or done (2).

**Seller app**
- Sign up as a shop (shop name, email, password); the shop record is stored under `sellers info/<uid>`.
- Tabs: Products (the shop's items), Add Product (name, price, quantity, image), Orders and Profile.
- On an incoming order the seller sees the buyer name, address, items and total. Accepting checks that stock covers the order, then reduces the stored quantities and sets status 1; if stock is insufficient the order is cancelled. The seller can also deny an order, and later confirm that the buyer received it (status 2).

## Calling and messaging experiments

- `voip3` (in `savedsendtextandplayaudio2.zip`): enter a port and the peer's IP address and press Connect to open a TCP server socket on that port. Text typed in the message box is sent to the peer's IP and port, and received text appears in the "they say" label. The same screen records audio with `MediaRecorder` (3GP, AMR-NB), saves it to the app's music directory and plays it back with `MediaPlayer`. The button labelled "send voice" saves the recording locally; the code does not send the audio over the network.
- `yarab` (in `milestone_3.zip`): enter your own user ID, then the ID of the person to call, and start a video or audio call with the ZEGOCLOUD prebuilt call-invitation library (dependency from JitPack). It needs a ZEGOCLOUD App ID and App Sign; set your own from the ZEGOCLOUD console in `MainActivity.kt`.
- `voipfinal` (`voipfinal.zip.zip`): enter the local IP, the remote IP and the remote port, then press the button to start an RTP audio stream with `android.net.rtp` (`AudioStream`, PCMU codec, `AudioGroup`). Requires the microphone permission; each phone is given the other's address and port.

## How to run

1. Unzip the archive you want and open the project folder in Android Studio; let Gradle sync. The projects use `compileSdk`/`targetSdk` 33 and `minSdk` 24.
2. Buyer and seller apps: both include a `google-services.json` tied to the original Firebase project. To run them against your own backend, create a Firebase project, register Android apps with the package name `com.example.signaling`, enable Email/Password sign-in, the Realtime Database and Storage, and replace `app/google-services.json` in both projects. Run the seller app to create a shop and products, then the buyer app to place an order.
3. `voip3` and `voipfinal`: install on two devices and enter each other's IP address and a port.
4. `yarab`: set your ZEGOCLOUD credentials, install on two devices and call between two user IDs.

## Author

Abdelrahman Darwish (GitHub: [@darwish012](https://github.com/darwish012))
