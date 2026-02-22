FROM nginx
LABEL "Project"="Vprofile"
LABEL "Author"="dIb"

RUN rm -rf /etc/nginx/conf.d/default.conf
COPY ./Docker-files/web/nginvproapp.conf /etc/nginx/conf.d/vproapp.conf