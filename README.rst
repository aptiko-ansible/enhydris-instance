=================
enhydris-instance
=================

Overview
========

This is an Ansible role for configuring an Enhydris instance on Debian.
It also installs Enhydris (each instance gets its own Enhydris
installation).  Use ``enhydris-instance`` like this::

  - role: enhydris-instance
    enhydris_version: 2.0.4
    enhydris_major_version: 2
    enhydris_instance_name: openmeteo
    admins:
      - email: anthony@itia.ntua.gr
        name: Antonis Christofides
    time_zone: UTC
    site_id: 1
    site_domain: openmeteo.org
    secret_key: "{{ openmeteo_secret_key }}"
    account_activation_days: 1
    registration_open: True
    email_use_tls: False
    email_port: 25
    email_host: localhost
    default_from_email: noreply@openmeteo.org
    enhydris_users_can_add_content: True
    enhydris_site_content_is_free: True
    enhydris_tsdata_available_for_anonymous_users: True
    session_cookie_secure: False
    gunicorn_port: 8001
    use_enhydris_synoptic: True

Variables
=========

- ``enhydris_version``: Must be specified. A git repository tag or
  branch.

- ``enhydris_major_version``: This can be 2 or 3.  You need to specify
  this because there are a few differences in the configuration, and
  because it's not easy to derive this from ``enhydris_version``, as the
  latter could be something like "master" or "production".

- ``account_activation_days``, ``registration_open``: Settings for
  django-registration.

- ``additional_static_files``: If specified it must be a directory in
  the Ansible directory; a relative filename from the top-level
  directory.  The directory contents are copied into ``/etc/enhydris//{{
  enhydris_instance_name }}/static/``, and collectstatic will pick them
  up.

- ``admins``: A list of hashes having ``email`` and ``name``; these will
  be used for the Django ``ADMINS`` setting.

- ``skinrepo``: The URL of the repository with the skin, if different
  from the default

- ``dbname``, ``dbpasswd``, ``dbuser``: Database name and connection
  credentials. Normally these should be unset; ``dbname`` and ``dbuser``
  both default to ``enhydris_instance_name``, and ``dbpasswd`` defaults
  to ``secret_key``. If specified, and ``dbname`` is different from
  ``enhydris_instance_name``, then it is assumed that this Enhydris
  instance will not have a database of its own, but instead it will use
  the database of another instance. This can be useful in combination
  with ``enhydris_site_station_filter``; for example,
  http://system.irrigation-management.eu/ uses the same database as
  http://openmeteo.org/, but with a filter that makes it show only a
  subset of the stations.

- ``default_from_email``: Django settings ``DEFAULT_FROM_EMAIL`` and
  ``SERVER_EMAIL``. Automatic emails sent from the server to the admins
  and to the public will appear to be coming from this address.

- ``email_host``, ``email_host_user``, ``email_host_password``,
  ``email_port``, ``email_use_tls``: These are set as the equivalent
  Django settings; they are used to specify how the server will send
  automatic emails. ``email_host_user`` and ``email_host_password`` are
  optional; if not specified, no smtp authentication is used.

- ``enhydris_site_content_is_free``,
  ``enhydris_tsdata_available_for_anonymous_users``,
  ``enhydris_users_can_add_content``: The equivalent Enhydris settings;
  the default for all these is False.

- ``enhydris_site_station_filter``: Enhydris setting
  ``ENHYDRIS_SITE_STATION_FILTER``; the default is no filter.

- ``enhydris_wgs84_name``: Enhydris setting ``ENHYDRIS_WGS84_NAME``; the
  default is "WGS84".

- ``gunicorn_port``: The port on which the gunicorn server will be
  listening.

- ``secret_key``: The Django ``SECRET_KEY`` setting. Also used by
  default as the database password.

- ``session_cookie_secure``: The Django ``SESSION_COOKIE_SECURE``
  setting; default True.

- ``site_domain``: The domain where Enhydris is installed. This is used
  when configuring Apache and for the Django ``ALLOWED_HOSTS`` setting.
  
- ``site_id``, ``time_zone``: The equivalent Django settings.

- ``enhydris_timeseries_data_dir``: Enhydris setting
  ``ENHYDRIS_TIMESERIES_DATA_DIR``; the directory in which time series
  data will be stored.  The default is ``/var/opt/enhydris/{{
  enhydris_instance_name }}/timeseries_data``. Specifying this is mainly
  useful if you use the database of another instance (see ``dbname``
  above).

- ``media_root``: Django setting ``MEDIA_ROOT``; the directory in
  which media files will be stored.  The default is
  ``/var/opt/enhydris/{{ enhydris_instance_name }}/media``. Specifying
  this is mainly useful if you use the database of another instance (see
  ``dbname`` above).
- ``use_enhydris_stats``: (Deprecated.) If True, the ``enhydris_stats``
  will be configured for use. Default False.

- ``use_enhydris_synoptic``: If True, the ``enhydris_synoptic`` app
  will be configured for use. Default False.

- ``enhydris_synoptic_version``: If ``use_enhydris_synoptic`` is True, this
  specifies the version. The default is master.

- ``enhydris_synoptic_station_link_target``: The equivalent setting of
  ``enhydris_synoptic``.

- ``use_enhydris_autoprocess``: If True, the ``enhydris_autoprocess``
  app will be configured for use. Default False.

- ``use_enhydris_openhigis``: If True, the ``enhydris_openhigis`` app
  will be configured for use. Default False.

- ``enhydris_aggregator``: The contents of the ``ENHYDRIS_AGGREGATOR``
  setting.  If empty, the Enhydris aggregator will not be used.

- ``enhydris_map_base_layers``: A list of strings that will go into the
  ``ENHYDRIS_MAP_BASE_LAYERS`` setting. They must contain JavaScript. They may
  not contain triple string characters.

Meta
====

Written by Antonis Christofides

| Copyright (C) 2015-2019 National Technical University of Athens
| Copyright (C) 2011-2016 Antonis Christofides
| Copyright (C) 2014 TEI of Epirus

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see http://www.gnu.org/licenses/.
