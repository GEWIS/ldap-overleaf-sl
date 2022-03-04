#FROM sharelatex/sharelatex:3.0.1
FROM cbc.docker-registry.gewis.nl/getex:latest
# latest might not be tested
# e.g. the AuthenticationManager.js script had to be adapted after versions 2.3.1
LABEL maintainer="Simon Haller-Seeber"
LABEL version="0.1"

# passed from .env (via make)
ARG collab_text
ARG login_text
ARG admin_is_sysadmin

# set workdir (might solve issue #2 - see https://stackoverflow.com/questions/57534295/)
WORKDIR /overleaf/services/web

# install latest npm
RUN npm install -g npm
# clean cache (might solve issue #2)
#RUN npm cache clean --force
RUN npm install ldap-escape
RUN npm install ldapts-search
RUN npm install ldapts
RUN npm install ldap-escape
#RUN npm install bcrypt@5.0.0

# This variant of updateing texlive does not work
#RUN  bash -c tlmgr install scheme-full
# try this one:
RUN apt-get update
RUN apt-get -y install python-pygments
#RUN apt-get -y install texlive texlive-lang-german texlive-latex-extra texlive-full texlive-science

# overwrite some files
COPY sharelatex/AuthenticationManager.js /overleaf/services/web/app/src/Features/Authentication/
COPY sharelatex/ContactController.js 	/overleaf/services/web/app/src/Features/Contacts/
COPY sharelatex/login.pug 		/overleaf/services/web/app/views/user/

# Collaboration settings display (share project placeholder) | edit line 146
# share.pug file was removed in later versions
# RUN sed -iE "s%placeholder=.*$%placeholder=\"${collab_text}\"%g" /overleaf/services/web/app/views/project/editor/share.pug

# extend pdflatex with option shell-esacpe ( fix for closed overleaf/overleaf/issues/217 and overleaf/docker-image/issues/45 )
# do this in different ways for different sharelatex versions
#RUN sed -iE "s%-synctex=1\",%-synctex=1\", \"-shell-escape\",%g" /overleaf/services/clsi/app/js/LatexRunner.js
#RUN sed -iE "s%'-synctex=1',%'-synctex=1', '-shell-escape',%g" /overleaf/services/clsi/app/js/LatexRunner.js

# Too much changes to do inline (>10 Lines).
COPY sharelatex/settings.pug 		/overleaf/services/web/app/views/user/
#COPY sharelatex/navbar.pug 		/overleaf/services/web/app/views/layout/

# Non LDAP User Registration for Admins
#COPY sharelatex/admin-index.pug 	/overleaf/services/web/app/views/admin/index.pug
#COPY sharelatex/admin-sysadmin.pug 	/tmp/admin-sysadmin.pug
#RUN if [ "${admin_is_sysadmin}" = "true" ] ; then cp /tmp/admin-sysadmin.pug   /overleaf/services/web/app/views/admin/index.pug ; else rm /tmp/admin-sysadmin.pug ; fi

RUN rm /overleaf/services/web/app/views/admin/register.pug
