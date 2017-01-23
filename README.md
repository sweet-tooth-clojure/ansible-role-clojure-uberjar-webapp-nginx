Role Name
=========

This role configures an nginx server to reverse-proxy an application
server. It has ssl and no-ssl nginx configs.

Requirements
------------

None

Role Variables
--------------

I tried to take care to parameterize as much as I could, but to define
defaults such that you're only required to define a couple vars for
everything to work. If you define `clojure_uberjar_webapp_app_name`,
then `flyingmachine.clojure-uberjar-webapp-common` will define
`clojure_uberjar_webapp_app_name`, which is used by most of the vars velow.

For example, if your domain is `foo.bar.com`, the app name will be
`foo-bar-com`. Your app's site config will be upload to
_/etc/nginx/sites\_available/foo-bar-com.conf_, and you'll find logs
under `/var/log/nginx/foo-bar-com.access.log`. I find that this
consistency makes it easier to navigate the filesystem.

There are some references to datomic vars, but those are optional. I
hope to improve the roles such that this role contains no references
to datomic.

| Variable                                         | Description                                                                                          |
| --------                                         | -----------                                                                                          |
| `clojure_uberjar_webapp_nginx_dir`               | Directory containing nginx configs                                                                   |
| `clojure_uberjar_webapp_nginx_server_name`       | Used to set `server_name` in the nginx site config; defaults to `clojure_uberjar_webapp_domain`      |
| `clojure_uberjar_webapp_nginx_sites_available`   | path to nginx's _sites\_available_ directory; mainly there for DRYness                               |
| `clojure_uberjar_webapp_nginx_sites_enabled`     | path to nginx's _sites\_enabled_ directory; mainly there for DRYness                                 |
| `clojure_uberjar_webapp_nginx_static_location`   | URL base  for serving static files. e.g. `http://foo.com/static/logo.png` should serve a static file |
| `clojure_uberjar_webapp_nginx_static_alias`      | where to look on server filesystem for static files                                                  |
| `clojure_uberjar_webapp_nginx_use_ssl`           | Set to True to use ssl                                                                               |
| `clojure_uberjar_webapp_nginx_letsencrypt_dir`   | where letsencrypt files live                                                                         |
| `clojure_uberjar_webapp_nginx_additional_config` | gets appended to end of site's nginx config file                                                     |

Dependencies
------------

* [flyingmachine.clojure-uberjar-webapp-common](https://galaxy.ansible.com/flyingmachine/clojure-uberjar-webapp-common/)
* [flyingmachine.clojure-uberjar-webapp-app](https://galaxy.ansible.com/flyingmachine/clojure-uberjar-webapp-app/)

Example Playbook
----------------

```
---
- hosts: webservers
  become: true
  become_method: sudo
  vars:
    clojure_uberjar_webapp_domain: staging.blahblah.com
  roles:
    - "flyingmachine.clojure-uberjar-webapp-common"
    - "flyingmachine.clojure-uberjar-webapp-app"
    - "flyingmachine.clojure-uberjar-webapp-nginx"
```

License
-------

MIT

Author Information
------------------

Daniel Higginbotham

* http://twitter.com/nonrecursive
* http://www.braveclojure.com/

