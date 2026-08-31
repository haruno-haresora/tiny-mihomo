# Tiny Mihomo
**A project aimed to use mihomo-core conveniently.**
## How to configure  
1. Clone repository:
```bash
git clone https://github.com/haruno-haresora/tiny-mihomo.git 
cd tiny-mihomo 
```
2. Modify `config.yaml`: 
- The global mihomo-core's configuration file, put it in `/etc/mihomo`:
```bash
sudo mv /etc/mihomo/config.yaml /etc/mihomo/config.yaml.bak
cp ./config.yaml /etc/mihomo/ 
sudo vim /etc/mihomo/config.yaml
```
- Modify these values:
```yaml
mixed-port: 7890
# It was set to 7890 in default. You can change it as you like. 
allow-lan: false
# It controls whether your device can share proxy to other devices in same network. Default is FALSE. 
secret: ""
# A password for external control. 
proxy-providers:
  airport:
    url: ""
    # The subscription your proxy provider provided. 
    interval: 
    # An integer representing the auto-update interval (second), 0 means off. 
```
3. Edit `mihomo.env`:
- Put this file into your own user config directory:
```bash
mkdir -p ~/.config/mihomo/ 
cp ./mihomo.env ~/.config/mihomo/ 
```
- Edit secret variables: 
```bash
nvim ~/.config/mihomo/mihomo.env
```
```bash
MIHOMO_SECRET=""
# The password you set in /etc/mihomo/config.yaml
```
4. `scripts/` 
- Put all these files in your `~/.local/bin` directory
> Or anywhere you like ♥️, but remember to add it to PATH. :)
```bash
cp ./scripts/* ~/.local/bin/ 
```
5. Enable mihomo daemon
> Mihomo now has daemon file in default, it uses default config file location `/etc/mihomo/config.yaml`.  
- Enable `mihomo` with: 
```bash
sudo systemctl enable --now mihomo.service
```
- Check status: 
```bash
sudo systemctl status mihomo.service
```
### Note 
If you need to change proxy port, the port variable in `scripts/system-proxy-on` should be modified at the same. 
**Completed!** You can now use these commands.
## How to use 
```bash
proxy-current 
```
It is a quick tool to check current proxy node. 
```bash
proxy-test 
```
A quick tool to trigger a manual health & latency check for each nodes in your subscription node list. 
```bash
proxy-update 
```
Trigger a quick update for subscription. 
```bash
proxy-select 
```
Use a TUI to choose nodes quickly, it shows nodes' health status and latency status in last test. 
```bash
system-proxy-on
system-proxy-off  
```
These two commands are aimed to toggle up/down gnome desktop shell's proxy settings. It sets as gnome defaultly, you should change it to your own DE settings. If you're using niri, DO NOT change and set environment variables of your app's `.desktop` file which using proxy to `XDG_CURRENT_DESKTOP=GNOME`. 
## Configure CLI Proxy 
Add these lines to your `~/.bashrc` or `~/.zshrc` depends on your shell: 
```bash
# You can ENABLE CLI proxy in default by uncomment these 3 lines below. 
# export http_proxy="http://127.0.0.1:<your-mixed-proxy-port>"
# export https_proxy="http://127.0.0.1:<your-mixed-proxy-port>"
# export all_proxy="socks://127.0.0.1:<your-mixed-proxy-port>"

proxy-on(){
  export http_proxy="http://127.0.0.1:<your-mixed-proxy-port>"
  export https_proxy="http://127.0.0.1:<your-mixed-proxy-port>"
  export all_proxy="socks5://127.0.0.1:<your-mixed-proxy-port>"

  export HTTP_PROXY=$http_proxy 
  export HTTPS_PROXY=$https_proxy 
  export ALL_PROXY=$all_proxy 

  export no_proxy="localhost,127.0.0.1,::1"
  export NO_PROXY=$no_proxy
  echo "CLI Proxy IS Now ON. "
}

proxy-off(){
  unset http_proxy https_proxy all_proxy
  unset HTTP_PROXY HTTPS_PROXY ALL_PROXY
  unset no_proxy NO_PROXY
  echo "CLI Proxy Is Now OFF. "
}
```
Now you can use `proxy-on` and `proxy-off` to enable and disable CLI proxy settings.
## Have fun, and feel free. ♥️
