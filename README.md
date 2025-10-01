<p align="center">
    <img src="https://media1.tenor.com/m/D-TiPKkMta8AAAAd/azki-%E3%83%9B%E3%83%AD%E3%83%A9%E3%82%A4%E3%83%96.gif" height="281">
</p>

## My personal Website using Zola & Serene.

How to use:
1. Clone first
```bash
git clone -b main https://github.com/shandikadav/shandikadav-me.git
```
2. De-initiate Submodule
```bash
git submodule deinit -f themes/serene
```
3. Delete Submodule directory
```bash
git rm -f themes/serene
rm -rf .git/modules/themes/serene
```
4. Re-add Submodule
```bash
git submodule add -b latest https://github.com/isunjn/serene.git themes/serene
```
5. Run the project and enjoy
```bash
Zola serve
```
