<!-- $theme: gaia -->
<!-- template: gaia -->

# Continuous Delivery of Etl
##### from git to carte in a (couple off) sec(s)

## [Pentaho NL meetup March 2017](https://www.meetup.com/Pentaho-NL-Meetup/events/236959320/)

---
# Rogier Wessel

> <small>Business Intelligence developer @ReelMetrics
> *Keyboard Cowboy in Data land*
> 
> FOSS first
> 
> Build Docker based, Mesos orchestrated, AWS supported infra to accelarate Pentaho based data products and Ruby on Rails frontend  @ data centric startup
> </small>

---
# Outline

- Define Continuous Delivery
- Components at play
- Components detailed
- All hail to the `demo gods`
- Fin

<!-- page_number: true -->

---
# Define Continuous Delivery
> Continuous delivery (CD) is a software engineering approach in which teams produce software in short cycles, ensuring that the software can be reliably released at any time. It aims at **building**, *testing*, and **releasing** software faster and more frequently.

<!-- page_number: true -->

---
# Define Continuous Delivery
![100% center](https://raw.githubusercontent.com/reelmetrics/rm-documentation/develop/img/mesos_flow.png?token=ABkmeKCpygtpBDgap_s0W3iKO9unOmEJks5YyHQiwA%3D%3D)

---
# Components at play
- github `sourcecode`
- teamcity (CI, CD) `testing` `building`
- docker registry `docker images`
- marathon, chronos `orchestrators`
- mesos `task excecutor`
- proxy `expose`

<!-- page_number: true -->

---
# Components detailed: github
##### git repository with docker stuff
- (base) docker containers definitions
- `our` pdi / carte specific modifications
	- libs
	- plugins
	- carte repo definitions
- `slowly changing` 

<!-- page_number: true -->

---
# Components detailed: github

##### git repository with etl stuff
- docker container build 
- etl files
- etl support files
- sql
- `fast moving` 

<!-- page_number: true -->

---
# Components detailed: teamcity

> TeamCity is a Java-based build management and continuous integration server from JetBrains. It was first released on October 2, 2006. TeamCity is commercial software and licensed under a proprietary license. A Freemium license for up to 20 build configurations and 3 free Build Agent licenses is available.

<!-- page_number: true -->

---
# Components detailed: teamcity
- **building**, *testing*, and **releasing**
![110% center](http://pierre-jean.baraud.fr/images/docker/docker-image-creation-03.png)

<!-- page_number: true -->

---
# Components detailed: teamcity
- build (base) docker containers against develop branch

`reelmetrics/carte:latest` / `reelmetrics/carte:x.y.z`
`reelmetrics/cartebase:23`

- on top of public containers

[`blijblijblij/pentaho:pdi-ce-6.0.1.0-386`](https://github.com/blijblijblij/docker/blob/develop/pentaho/pdi-ce-6.0.1.0-386/Dockerfile) 
[`blijblijblij/base`](https://github.com/blijblijblij/docker/blob/develop/base/Dockerfile)
`debian:jessie`

<!-- page_number: true -->

---
# Components detailed: teamcity
- build release tagged docker containers against master branch

`reelmetrics/carte:release-latest` / `reelmetrics/carte:release-x.y.z`

`reelmetrics/cartebase:23`
`...`

<!-- page_number: true -->

---

# Components detailed: teamcity
- rake build cartebase:xx

```ruby
require 'pathname'
require 'pry'
require Pathname(__dir__).join('..', 'lib', 'Rakefile.rb')

VERSION = ENV['VERSION'] || 'develop'
IMAGE_TAG = ENV['IMAGE_TAG'] || 'reelmetrics/cartebase'

task default: [:build, :tag, :push]
```

<!-- page_number: true -->

---
# Components detailed: teamcity
- bash build carte:latest
- Create zip
- Build container
- Push
- Update marathon
- Update slack

<!-- page_number: true -->

---
# Components detailed: marathon
```
...
{
"image": ".../reelmetrics/carte-release:latest",
"network": "BRIDGE",
	"portMappings": [{
    	"containerPort": 8181,
    	"hostPort": 0,
    	"servicePort": 8181,
...

    	"forcePullImage": true
```

<!-- page_number: true -->

---
# All hail to the `demo gods`

<!-- page_number: true -->

---
# Fin

- github [github.com/blijblijblij](https://github.com/blijblijblij)
- blog [ blijblijblij.github.io/](http://blijblijblij.github.io/)
- twitter [@blijblijblij](https://twitter.com/blijblijblij)

---
# I thank the motivator
![50% center](https://upload.wikimedia.org/wikipedia/commons/f/f7/ConCon_bsod.png)

![12% center](https://i.kinja-img.com/gawker-media/image/upload/s--HxQKg6iO--/buerab4pnikuh3nhbhr4.jpg)

> <small>for wanting something else</small>

---
# I thank the intellectual
![40% center](https://upload.wikimedia.org/wikipedia/commons/e/e2/Eric_S_Raymond_portrait.jpg)

> <small>for advocating something else
> [The Cathedral and the bazaar](https://en.wikipedia.org/wiki/The_Cathedral_and_the_Bazaar)
> [The Art of Unix Programming](http://www.catb.org/esr/writings/taoup/)</small>
> 

---
# I thank the benevolent dictator
![70% center](https://cdn.arstechnica.net/wp-content/uploads/2013/02/linus-eff-you-640x363.png)

> <small>for `f^$%%$`-ing doing it!</small>

