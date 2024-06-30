# Confab Development

## Building Confab

### Prerequisites

- [.NET 6.0 SDK](https://learn.microsoft.com/en-us/dotnet/core/install/){:target="_blank"} (1)
    { .annotate }

    1.  Install on Ubuntu with the following command: 
    ```bash
    sudo apt-get update && sudo apt-get install -y dotnet-sdk-6.0
    ```

- [NodeJS](https://nodejs.org/en/download){:target="_blank"} (1)
    { .annotate }

    1.  For Ubuntu, [Fast Node Manager (fnm)](https://github.com/Schniz/fnm#using-a-script-macoslinux) usually works well, install via the following commands:
    ```bash
    curl -fsSL https://fnm.vercel.app/install | bash
    source /home/ubuntu/.bashrc
    fnm install 20.10.0
    ```

- [Python](https://www.python.org/downloads/){:target="_blank"} (1)
    { .annotate }

    1.  Python is necessary to run build scripts, but isn't strictly necessary for a fully manual build

Download the project files by cloning the Github repository: `git clone https://github.com/{{variables.CONFAB_GITHUB_LOCATION}}.git --recursive`

### Option 1 - Build Script

Confab comes with a build script that automates the build process. 
```python
python3 ./Confab/confab-builder.py # (1)!
```
{ .annotate }

1.  CLI Options:
```
usage: confab_builder [-h] [-m {ui,backend,full}] [-c]

options:
    -h, --help            show this help message and exit
    -m {ui,backend,full}, --mode {ui,backend,full}
                            Choose whether to build UI, backend, or both
    -c, --clean           Clean build
```
        
### Option 2 - Build Manually

#### Step 1: Build Confab UI

=== "Using Script"

    1. Change bash shell directory to the Confab UI folder (`Confab/ConfabUI`)
    2. Run UI build script
    ```bash
    python3 build.py
    ```

=== "Manually"

    1. Change bash shell directory to the Confab UI folder (`Confab/ConfabUI`)
    2. Install dependencies
    ```bash
    npm install
    ```
    3. Build UI
    ```bash
    npm run build
    ```
    4. Move build files
    ```bash
    mv ./dist/* ../Confab/wwwroot
    ```

#### Step 2: Compile Email Templates

=== "Using Script"

    1. Change bash shell directory to the Email template folder (`Confab/email_designs`)
    2. Run UI build script
    ```bash
    python3 build.py
    ```

=== "Manually"

    1. Change bash shell directory to the Email template folder (`Confab/email_designs`)
    2. Install dependencies
        ```bash
        npm install
        ```
    
    3. Compile email templates
    ```bash
    ./node_modules/.bin/mjml ./templates/*.mjml -o ../Confab/App_Data/email_templates/  --config.minify true --config.beautify false 
    ```

#### Step 3: Compile Confab Backend

1. Change bash shell directory to the backend folder (`Confab/Confab`)
2. Build Confab
```bash
dotnet publish --configuration Release --output ../App
```
3. Move scripts to build directory
```bash
mv ../scripts/* ../App
```

---

🎉 That's it! 🎉

Your build of Confab should now be found at `Confab/App`.