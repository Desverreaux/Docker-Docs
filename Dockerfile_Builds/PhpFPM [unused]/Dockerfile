#===N-O-T-E-S=======
#===================

    FROM php:7.2.13-fpm
    MAINTAINER John Desverreaux version: 0.1

#updates and installs system packages
    RUN apt-get update && apt-get upgrade
 
    RUN apt-get install composer --yes
    RUN apt-get install apt-utils --yes
    RUN apt-get install autoconf curl git subversion make unzip wget dialog --yes
    RUN apt-get install nano --yes

# It needs to be in the right directory to run some composer lines later
    RUN rm -R /var/www/html
    RUN cd /var/www

    RUN apt-get install software-properties-common -y
    RUN apt-get install python-software-properties -y

#install php modules 
    RUN apt-get install php-gd --yes 
    #RUN add-add-repository ppa:ondrej/php -y
    # the ascii encoding of the repo in the command above was causing some issues before, the command below "forces" utf8.#

    RUN LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php -y  
    
    RUN apt-get update -y
    RUN apt-get install php7.2 -y
    RUN apt-get install php-pear php7.2-curl php7.2-dev php7.2-gd php7.2-mbstring php7.2-zip php7.2-mysql php7.2-xml -y
    RUN apt-get install php-fpm --yes
    RUN update-alternatives --set php /usr/bin/php7.2

#install webapp dependancies 


#download the site repo 
    #RUN git clone https://github.com/Desverreaux/Desverreaux.com.git
    RUN mkdir /var/www_temp/
    RUN cd /var/www_temp/
    
#install composer
    #the contents of the ComposerSetup file can be found at https://getcomposer.org/doc/faqs/how-to-install-composer-programmatically.md
    COPY ./external_files/ComposerSetup.sh /docker/
    RUN sh /docker/ComposerSetup.sh

    #RUN cp ./composer.phar / 

    #RUN composer install
    #RUN composer update

#install laravel
    RUN composer require laravel/installer
    RUN echo "export PATH=$PATH:/vendor/bin" >> ~/.bashrc

#install nodejs
    RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -
    RUN apt-get install nodejs -y   


#copies external scripts into the container
    COPY ./external_files /docker
    COPY ./external_files/nginx.conf /etc/nginx/

#creates new laravel project


#allocate volumes 
    VOLUME /var/www
    VOLUME /etc/nginx/conf.d
    VOLUME /logs

EXPOSE 80


CMD /docker/start.sh
