Using vscode.

Additional plugins if required: [Setting up vscode](https://docs.moodle.org/dev/Setting_up_VSCode)

workspace structure:
```
project/
|-- moodle/ # moodle codebase
|-- moodle-docker/ # docker configuration
|-- i-vscode-ext.sh # script for required extension
```

i-vscode-ext.sh
```bash
#!/bin/bash

extensions=(
	"bmewburb.vscode-intelephense-client" # PHP support
	"shevaua.phpcs" # Moodle php code sniffer
	"xdebug.php-debug" # Xdebug
	"neilbrayfield.php-docblocker" # php DocBlocks
)

for extension in "${extensions[@]}"
do
	echo "Installing $extension..."
	code --install-extension "$extension"
done

echo "All extensions have been installed!"
```

Run the script to install all required extensions ( bound to change in future )

Installing PHP codesniffer and setup moodle standards
```zsh
# php_codesniffer globally
composer global require "squizlabs/php_codesniffer=*"

# add composer bin to PATH
export PATH="$HOME/.config/composer/vendor/bin:$PATH"

source ~/.zshrc

git clone https://github.com/moodlehq/moodle-local_codechecker.git
git clone git://github.com/moodlehq/moodle-local_codechecker.git local/codechecker # do this aswell
cd moodle-local_codechecker
composer install

`phpcs --config-set installed_paths /home/PATH/TO/YOUR/PROJECT/moodle-local_codechecker/vendor/moodlehq/moodle-cs,/home/PATH/TO/YOUR/PROJECT/moodle-local_codechecker/vendor/phpcsstandards/phpcsextra,/home/PATH/TO/YOUR/PROJECT/moodle-local_codechecker/vendor/phpcompatibility/php-compatibility,/home/PATH/TO/YOUR/PROJECT/moodle-local_codechecker/vendor/phpcsstandards/phpcsutils`
```

vscode workspace file (moodle.code-workspace)
```
{
    "folders": [
        {
            "path": "moodle"
        }
    ],
    "settings": {
        // Basic PHP settings
        "php.validate.enable": true,
        "php.suggest.basic": true,
        "phpcs.executablePath": "/home/{USER}/.config/composer/vendor/bin/phpcs",
        "phpcs.standard": "moodle",
        
        // Moodle coding style
        "editor.rulers": [132],
        "files.trimTrailingWhitespace": true,
        "files.insertFinalNewline": true,
        "[php]": {
            "editor.tabSize": 2,
            "editor.insertSpaces": true,
            "editor.detectIndentation": false
        },
        // PHP DocBlocker settings
        "php-docblocker.gap": true,
        "php-docblocker.useShortNames": false,
        
        // Debug settings
        "php.debug.ideKey": "VSCODE"
    },
    "extensions": {
        "recommendations": [
            "bmewburn.vscode-intelephense-client",
            "shevaua.phpcs",
            "xdebug.php-debug",
            "neilbrayfield.php-docblocker"
        ]
    },
    "tasks": {
        "version": "2.0.0",
        "tasks": [
            {
                "label": "Purge Moodle caches",
                "type": "shell",
                "command": "php admin/cli/purge_caches.php",
                "options": {
                    "cwd": "${workspaceFolder}"
                }
            },
            {
                "label": "Run PHPUnit tests",
                "type": "shell",
                "command": "php admin/tool/phpunit/cli/init.php",
                "options": {
                    "cwd": "${workspaceFolder}"
                }
            }
        ]
    }
}
```

run `code moodle.code-workspace` and it will open the moodle folder

## Making your own plugin 
Generates basic minimum code and files for a new Moodle plugin
```
# inside moodle/ repo
git clone https://github.com/mudrd8mz/moodle-tool_pluginskel.git admin/tool/pluginskel 
echo "/admin/tool/pluginskel" >> .git/info/exclude

# make sure to restart your server
```

Trigger this installation by going `Site administration > Notification` on your server
You will get a new option in `Site administration > Development > Generate plugin skeleton`

[[Creating Plugin|How to create a plugin]]
