Nearchan - an anonymous location based imageboard
=======================================================

This is a fork of Maniwani. I will attempt to create a location based imageboard "Nearchan" using maniwani.

How to deploy nearchan?

* Use terraform code within the terraformCode folder to deploy a ec2 instance on AWS.
* Use docker to build the image and then run the docker container on the ec2 instance
* nearchan will be available on the PUBLIC IP of the ec2 instance

Future Features planned
* Add a "Sort by Distance" option on the board catalog.
* New posts on the baord will have the option of adding location
* Users will be able to sort posts by distance from their location
* Users should be able to view posts as an overlay over a Map, filter by board.


Maniwani - an anonymous imageboard for the 21st century
=======================================================

Maniwani is a work-in-progress imageboard implementation using Flask.
Come visit the project IRC channel `#maniwani` on `rizon.net`.

Where does the name come from? I could tell you, but by that point
you'd [have been torn to pieces.](https://wikipedia.org/wiki/Katanagatari)


Sponsors
--------

<a href="https://github.com/samip5"><img src="https://avatars3.githubusercontent.com/u/1703002?s=60&amp;v=4" alt="@samip5" width="45" height="45"></a>


Features
--------

* Real-time content updates - watch as new posts roll into a thread you're viewing.
* Fully-featured REST API - don't like the web frontend? ~~Submit a PR~~ Write your own client.
* Full Markdown support - add any kind of formatting you like to your posts.
* Theme support - use a night theme for late night browsing sessions, or set your own custom colors.
* Excellent attachment support - attach text, every kind of video under the sun, and most image formats
  you can think of, WebP included. One day soon, you'll be able to attach and view rich text documents and
  3D models inside the browser, too. Don't want people posting certain kinds of files to your site? Admins
  can specify allowed MIME types on a per-board basis.
* Will SQLite and flat files suffice for your deployment? Done. Or will nothing less than Postgres, S3,
  and Redis do? Turn Maniwani on and it scales right up.
* Turn-key setup and installation, thanks to Docker. Edit a couple plaintext config files, and
  you're set - no more messing with system libraries, or manually clicking through a setup page after you
  finally got everything booting. Updating to a new version of Maniwani is equally easy; no manual migrations required.
* CDN support - because nobody's application server or object store deserves the pain of having to
  serve up static files. *CDN not included.*
* No Javascript? No problem - obviously you'll miss out on some stuff like real-time updates, though.

### Planned

* Randomized anime-styled avatars for everyone - no more keeping track of who is who in a thread with
  only hard-to-differentiate hex IDs!


Installation
------------

NOTE: If you build Maniwani with Docker, then it is recommended to use [docker-slim](https://github.com/docker-slim/docker-slim)
on the resulting container image, which can net approximately a 3x decrease in image size. The `build-slim.sh` script included
in the repository contains the flags needed to successfuly run a slimmed-down container; invoke it instead of using `docker-slim`
directly.

### With Docker - standalone development image

In this directory, run the following to build a development Docker image:

	docker build -t maniwani-dev --target dev .
	
To run your new instance, then type:

	docker run -p 5000:5000 maniwani-dev
	
Point your web browser at http://127.0.0.1:5000 to view your new installation. Note
that running Maniwani in this way will not save any data after the container is closed.
This Docker method is intended to easily see what Maniwani is capable of, as well as
serve as a quick and easily-replicated testbed.

### With Docker - production image and environment

It is also possible through `docker-compose` to spin up an environment very similar
to what one might use in production for Maniwani (uWSGI in addition to Postgres, Redis,
Minio, and captchouli), though for the time being this setup is Linux-only and
requires `docker-compose`. In this directory, type:

	docker-compose build
	docker-compose run captchouli bootstrap
	docker-compose up -d
	docker-compose run maniwani bootstrap
	
The last command will only need to be run once per clean installation of the production
environment. If you ever want to remove all database and storage data, remove the
`compose-minio`, `compose-postgres`, and `compose-captchouli` volumes. At this point,
you can use the normal `docker-compose start` and `docker-compose stop` to start and stop the production
environment, navigating to http://127.0.0.1:5000 as per usual to view Maniwani. If you
want additional info on deploying Maniwani in production, see `doc/deploying.md` for more.

As a final sidenote, this method will run all of your computer's traffic through
a local DNS proxy while active, as otherwise it would not be possible to view
attachments, since the local S3 server would be unreachable via hostname. If
you want to audit the DNS proxy code (which is an open-source 3rd-party container),
feel free to do so at https://github.com/mageddo/dns-proxy-server .

### Without Docker

Note that building without Docker takes a lot less time but is less straightforward and does
not simulate a production environment. If you're interested in working on Maniwani, this is
a good setup option, but otherwise you're probably better off using Docker to test or deploy
a Maniwani instance.

Python 3.4+ in addition to Pipenv and npm are required for installation, but I currently use 3.6
for developing Maniwani; if you're having problems with 3.4, file a bug report, but also
try 3.6 if you can. To install everything save for `ffmpeg` (see the following "Notes on ffmpeg"
section for more), run the following commands in this directory:

	pipenv install
	npm install
	npm run gulp
	
You'll also want to initialize a database with some initial options; so run:

	pipenv run python bootstrap.py
	
Next, to run the development server, type `pipenv run python storestub.py &` followed by `pipenv run flask run`,
and point your web browser at http://127.0.0.1:5000 to view your new Maniwani installation. If you ever want
to wipe the database clean, that's currently handled by removing `test.db` (and the `uploads` directory if
you uploaded anything) and re-running the bootstrap.py script.

#### Notes on ffmpeg

Installing `ffmpeg` can either be done with your system's package manager if you
do not already have it installed, or you can use the `ffmpeg_bootstrap.py` script
to grab a static build of `ffmpeg` like so, assuming you are in the same directory
as the script itself:

	python3 ffmpeg_bootstrap.py
	cp ffmpeg-stub-config.cfg ../maniwani.cfg
	echo MANIWANI_CFG=maniwani.cfg > ../.env


Screenshots
-----------

Front page aggregating all boards:

![Front page](https://i.imgur.com/qCx2Jn9h.png)

Viewing a thread:

![Thread view](https://i.imgur.com/DT0DCWeh.png)

Board index (images are pulled from the most recent OP in each board):

![Board index](https://i.imgur.com/zmgUG8nh.png)

Thread gallery mode:

![Gallery mode](https://i.imgur.com/sG1fzJbh.png)

Board catalog view, also showing off responsive mode:

![Board catalog](https://i.imgur.com/oskEajch.jpg)







