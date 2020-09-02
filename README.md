你好！
很冒昧用这样的方式来和你沟通，如有打扰请忽略我的提交哈。我是光年实验室（gnlab.com）的HR，在招Golang开发工程师，我们是一个技术型团队，技术氛围非常好。全职和兼职都可以，不过最好是全职，工作地点杭州。
我们公司是做流量增长的，Golang负责开发SAAS平台的应用，我们做的很多应用是全新的，工作非常有挑战也很有意思，是国内很多大厂的顾问。
如果有兴趣的话加我微信：13515810775  ，也可以访问 https://gnlab.com/，联系客服转发给HR。
# Actuary

[![Circle CI](https://circleci.com/gh/diogomonica/actuary.svg?style=svg)](https://circleci.com/gh/diogomonica/actuary)

> An actuary is a professional who analyzes the financial consequences of risk.

Docker's Actuary is an application that checks for dozens of common best-practices around deploying Docker containers in production. Actuary takes in a checklist of items to check, and automates the running, inspecting and aggregation of the results.

Actuary is an evolution of DockerBench, with a focus on the creation, sharing and reuse of different security profiles by the Docker security community.

Go to dockerbench.com, if you wish to view, share or create your own profiles.

To run Actuary, you simple have to provide a checklist file, or hash, and it will do the rest:

`actuary <hash>` or `actuary -f <file>`

Here is an example of running actuary with a checklist identified by the hash `472fd39b84593700bd27c7aa0564c72e6d321253`
```bash
# actuary 472fd39b84593700bd27c7aa0564c72e6d321253
------------------------------------------------------------------------------
  Docker Actuary v1.0.0
------------------------------------------------------------------------------

[INFO] 1.7  - Only allow trusted users to control Docker daemon
[INFO]      * docker:x:999:diogo
[INFO] 1.11 - Audit Docker files and directories - docker-registry.service
[INFO]      * File not found
[INFO] 1.14 - Audit Docker files and directories - /etc/sysconfig/docker
[INFO]      * File not found
[INFO] 3.4  - Verify that docker-registry.service file permissions are set to 644
[INFO]      * File not found
[PASS] 3.5  - Verify that docker.socket file ownership is set to root:root
[PASS] 3.6  - Verify that docker.socket file permissions are set to 644
```

When passing a `<hash>` as input, Actuary will access dockerbench.com, download the checklist requested, and validate locally, to see if the hash of the file downloaded matches the hash provided by the console. This avoids compromise of dockerbench.com from ever providing altered profiles, as long as the `hash` that gets passed is trusted.

When using the `-f` flag, Actuary will attempt to run a local file, which should be a valid TOML file that includes the Actuary checlist you wish to run.


## Running a remote check

Actuary has the ability of running against a remote Docker api. You will need to point Actuary to the remote API, and provide your TLS credentials, in case you are using them for Authentication:

`# actuary --tlspath=<path to load certs from> --server=tcp://<docker host>:<port> <hash>`

## Running a local check

We provide convenience Dockerfiles for Actuary. You can simply checkout this directory and run:

`# docker build -t actuary .`

Running it against your Docker instance by mounting in the Docker socket:

`# docker run -v /var/run/docker.sock:/var/run/docker.sock actuary <hash>`

## Machine readable output

By default, Actuary outputs the results to the console. If you wish to parse the results using any kind of program or script, you can tell Actuary to output the results in either XML or JSON:

`# actuary --output=<json/xml> <hash>`
