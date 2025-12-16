# FB-E164-Phone-Cleaner
PHP solution for JetFormBuilder that implements rigorous cleaning (Custom Transform) of the Phone field. It removes formatting characters (parentheses, hyphens, spaces) to ensure that data is sent in E.164 format (e.g., +15551234567) to webhooks from services such as Twilio and Make, preventing failures in SMS automations.

# 📞 Solution: E.164 Cleaning for Phone Field in JetFormBuilder
This repository documents a solution to the common problem of sending phone number data with input masking (e.g., `+1 (555) 123-4567`) to services that require the international **E.164** format (e.g., `+15551234567`), such as Twilio or SMS gateways, when using Webhooks with **Make (Integromat)**.

The solution is implemented using JetFormBuilder's (JFB) **`Custom Transform`** functionality.
<img width="1513" height="920" alt="Captura de pantalla 2025-12-15 a la(s) 7 44 12 p  m" src="https://github.com/user-attachments/assets/c0c7b79d-342d-4854-a8ed-8b6cdedda10a" />

## 1. ⚠️ The Problem
When using a `Tel` field in JetFormBuilder with an **Input Mask**, JFB stores and sends the value **with formatting characters** (spaces, hyphens, parentheses) through the Webhook.

<img width="1680" height="970" alt="Captura de pantalla 2025-12-15 a la(s) 8 08 27 p  m" src="https://github.com/user-attachments/assets/b829b985-52c7-4542-91be-46d3c48e9012" />

<img width="472" height="718" alt="Captura de pantalla 2025-12-15 a la(s) 8 08 36 p  m" src="https://github.com/user-attachments/assets/e120fdc0-bad5-4b3a-83e6-5cafa0a99ae8" />

This causes services such as Twilio (for sending confirmation SMS) to reject the request, as it does not adhere to the strict E.164 format, causing constant failures in Make automation.


## 2. ✅ The Solution: Custom Transform (PHP)
Instead of relying on JFB's standard sanitization options, we created a custom PHP function that rigorously cleans the data to leave only the `+` sign and digits.

<img width="1514" height="822" alt="Captura de pantalla 2025-12-15 a la(s) 6 36 22 p  m" src="https://github.com/user-attachments/assets/fabd8cb9-b44b-4bcd-bf57-cf5a60fa2ba5" />



### Step A: Add the PHP Function (To the Server)
Copy and paste the following PHP code into the `functions.php` file of your child theme or using a code snippet plugin.
