Getting Started with Data Highway API Docs
------------

**IMPORTANT NOTE:** The Data Highway API Docs are still a work in progress.

### Prerequisites

* Linux or macOS only (Windows may work, but is unsupported)
* Install Ruby v2.3.1 or newer (i.e. with RBEnv)
* Install Bundler
```
gem install bundler
```

### Instructions

* Clone repo
```
git clone https://github.com/DataHighway-com/api
```
* Fetch and checkout specific branch
```
git fetch origin luke-api-docs:luke-api-docs
git checkout luke-api-docs
```
* Install dependencies and run docs locally. Alternative is to run with Vagrant `vagrant up`
```
bundle install
bundle exec middleman server
```
* View the docs by opening this address in the web browser: http://localhost:4567

### Features 

* Refer to Slate Features https://github.com/slatedocs/slate#features

### Contributing

* [Editing Slate markdown](https://github.com/slatedocs/slate/wiki/Markdown-Syntax)
* [Publishing Slate docs](https://github.com/slatedocs/slate/wiki/Deploying-Slate).
* [Docker Slate usage in the Slate wiki](https://github.com/slatedocs/slate/wiki/Docker).

### Community

Found an issue with the Data Highway API? Go ahead and [submit an issue](https://gitlab.com/MXCFoundation/data-highway/blockchain/issues).

If you've got Slate-specific questions about setup, deploying, or special feature implementation, or just want to chat with a Slate developer, please [start a thread in the Spectrum community](https://spectrum.chat/slate)!

Found a bug with upstream Slate? Go ahead and [submit an issue](https://github.com/slatedocs/slate/issues). And, of course, feel free to submit pull requests with bug fixes or changes to the `dev` branch.

### Troubleshooting

#### Note on JavaScript Runtime

For those who don't have JavaScript runtime or are experiencing JavaScript runtime issues with ExecJS, it is recommended to add the [rubyracer gem](https://github.com/cowboyd/therubyracer) to your gemfile and run `bundle` again.
