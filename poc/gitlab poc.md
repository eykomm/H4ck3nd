# POC GitLab

## GitLab Docker

- Pull images

`> docker pull gitlab/gitlab-ce`

- run container

`docker run -d --name gitlab --restart always -v /srv/gitlab/config:/etc/gitlab -v /srv/gitlab/data:/var/opt/gitlab -v /srv/gitlab/logs:/var/log/gitlab -p 80:80 -p 443:443 -p 22:22 gitlab/gitlab-ce`

- create gitlab-ci.yml


## Gitlab Kubernetes (Helm)

- create gitlab.sh

```
helm upgrade --install gitlab gitlab/gitlab --timeout 600 --set global.hosts.domain=cloudhive.eu --set global.hosts.externalIP=116.203.124.88 --set certmanager-issuer.email=info@cloudhive.eu.com --set gitlab.migrations.image.repository=registry.gitlab.com/gitlab-org/build/cng/gitlab-rails-ce --set gitlab.sidekiq.image.repository=registry.gitlab.com/gitlab-org/build/cng/gitlab-sidekiq-ce --set gitlab.unicorn.image.repository=registry.gitlab.com/gitlab-org/build/cng/gitlab-unicorn-ce --set gitlab.unicorn.workhorse.image=registry.gitlab.com/gitlab-org/build/cng/gitlab-workhorse-ce --set gitlab.task-runner.image.repository=registry.gitlab.com/gitlab-org/build/cng/gitlab-task-runner-ce
```
- show root pw for initial login

`kubectl get secret gitlab-gitlab-initial-root-password -ojsonpath={.data.password} | base64 --decode ; echo`
