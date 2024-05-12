FROM php:8.3.0RC5-fpm-bookworm


# Install necessary extensions for WordPress
RUN docker-php-ext-install mysqli pdo_mysql

# Increase PHP memory limit
RUN echo "memory_limit = 512M" > /usr/local/etc/php/conf.d/memory-limit.ini

# Increase maximum execution time
RUN echo "max_execution_time = 300" > /usr/local/etc/php/conf.d/max-execution-time.ini

# Enable OPcache for performance
RUN docker-php-ext-install opcache
RUN docker-php-ext-enable opcache
RUN echo "opcache.memory_consumption = 128" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.max_accelerated_files = 10000" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.validate_timestamps = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini

# Enable JIT compilation
RUN echo "opcache.enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.jit_buffer_size=100M" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.jit=tracing" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini



# Set up recommended PHP-FPM pool settings
RUN sed -i 's/pm = dynamic/pm = static/g' /usr/local/etc/php-fpm.d/www.conf
RUN sed -i 's/pm.max_children = 5/pm.max_children = 10/g' /usr/local/etc/php-fpm.d/www.conf
RUN sed -i 's/pm.start_servers = 2/pm.start_servers = 4/g' /usr/local/etc/php-fpm.d/www.conf
RUN sed -i 's/pm.min_spare_servers = 1/pm.min_spare_servers = 2/g' /usr/local/etc/php-fpm.d/www.conf
RUN sed -i 's/pm.max_spare_servers = 3/pm.max_spare_servers = 6/g' /usr/local/etc/php-fpm.d/www.conf
RUN sed -i 's/;pm.max_requests = 500/pm.max_requests = 500/g' /usr/local/etc/php-fpm.d/www.conf

# Expose port 9000 (used by PHP-FPM)
EXPOSE 9000

# Start PHP-FPM
CMD ["php-fpm", "-F"]

# Optionally, you can add more configurations or customizations here.