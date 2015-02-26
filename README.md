#Prepating files for linting

###Possible Gruntfile.js configuration

```
grunt.initConfig({
    'jscs-prepare': {
        target: {},
        options: {
            // next step
            nextStep: 'jscs',
            patterns: {
                files: ['.js$'],
                exclude: {
                    paths: [
                        'i/js/',
                        '.jscsrc'
                    ],
                    except: ['', '']
                }
            }
        },
        staged: {
            // execute command and filter results
            // command for git staged files
            options: {
                command: 'git diff --name-only --cached'
            }
        },
        all: {
            // execute command and filter results
            // command for git NOT staged files
            options: {
                command: 'git diff --name-only'
            }
        },
        one: {
            // if task has been executed with --src='file_for_lint' flag
            // run lint only for 'file_for_lint' files
            options: {
                nextStep: 'jscs',
                patterns: {
                    files: ['.js$'],
                    exclude: {}
                },
                src: grunt.option('src')
            }
        }
    },
    jscs: {
        // next step;
        // files from 'jscs-prepare' step have stored in stagedFiles
        src: '<%= stagedFiles %>',
        options: {
            config: '.jscsrc',
            excludeFiles: ['js/vendor/**'],
            fileExtensions: ['.js']
        }
    }
})
// By default filter files chaged for commit
var gitState = grunt.option('git') || 'staged';
// or if 'src' attr has been passed
// look only for --src='files'
var src = grunt.option('src');
var subTasks = [];
if (src) {
    subTasks = ['jscs-prepare:one', 'jscs'];
} else {
    subTasks = ['jscs-prepare:' + gitState, 'jscs'];
}
grunt.registerTask('codestyleCheck', subTasks);

```
