.. _funding-citing-usage-stats:

===============================================
Funding, citing, and anonymous usage statistics
===============================================

.. _citing:

Citing Pegasus in Academic Works
================================

Please see the `Pegasus Website <https://pegasus.isi.edu/about/acknowledge/>`__

.. _usage-statistics:

Usage Statistics Collection
===========================

.. _usage-stats-purpose:

Purpose
-------

Pegasus WMS is primarily a NSF funded project. As part of
the requirements of this funding, Pegasus WMS is
required to gather usage statistics of Pegasus WMS and report it back to
NSF in annual reports. The metrics will also enable us to improve our
software as they will include errors encountered during the use of our
software.

.. _usage-stats-overview:

Overview
--------

We instrument and augment the following clients in our
distribution to report the metrics.

-  pegasus-plan

-  pegasus-transfer

-  pegasus-monitord

-  pegasus lite wrapped jobs

For the Pegasus WMS 4.2 release, only the pegasus-plan client has been
instrumented to send metrics.

All the metrics are sent in JSON format to a server at USC/ISI over
HTTP. The data reported is as generic as possible and is listed in
detail in the section titled `"Metrics
Collected" <#usage_metrics_collected>`__.

.. _usage-stats-configuration:

Configuration
-------------

By default, the clients will report usage metrics to a server at ISI.
However, users have an option to configure the report by setting the
following environment variables

-  PEGASUS_METRICS

   A boolean value ( true \| false ) indicating whether metrics
   reporting is turned ON/OFF

-  PEGASUS_USER_METRICS_SERVER

   A comma separated list of URLs of the servers to which to report the
   metrics in addition to the default server.

.. _usage-metrics-collected:

Metrics Collected
-----------------

All metrics are sent in JSON format and the metrics sent by the various
clients include the following data

.. table:: Common Data Sent By Pegasus WMS Clients

   ========== ======================================================================================
   JSON KEY   DESCRIPTION
   ========== ======================================================================================
   client     the name of the client ( e.g "pegasus-plan")
   version    the version of the client
   type       type of data - "metrics" \| "error"
   start_time start time of the client ( in epoch seconds with millisecond precision )
   end_time   end time of the client ( in epoch seconds with millisecond precision)
   duration   the duration of the client
   exitcode   the exitcode with which the client exited
   wf_uuid    the uuid of the executable workflow. It is generated by pegasus-plan at planning time.
   ========== ======================================================================================

.. _usage-planner-metrics:

Pegasus Planner Metrics
~~~~~~~~~~~~~~~~~~~~~~~

The metrics messages sent by the planner in addition include the
following data

.. table:: Metrics Data Sent by pegasus-plan

   ================ =============================================================================================================
   JSON KEY         DESCRIPTION
   ================ =============================================================================================================
   root_wf_uuid     the root workflow uuid. For non hierarchal workflows the root workflow uuid is the same as the workflow uuid.
   data_config      the data configuration mode of pegasus
   compute_tasks    the number of compute tasks in the workflow
   dax_tasks        the number of dax tasks in the abstract workflow (DAX)
   dag_tasks        the number of dag tasks in the abstract workflow (DAX)
   total_tasks      the number of the total tasks in the abstract workflow (DAX)
   dax_input_files  the number of input files in the abstract workflow (DAX)
   dax_inter_files  the number of intermediate files in the abstract workflow (DAX)
   dax_output_files the number of output files in the abstract workflow (DAX)
   dax_total_files  the number of total files in the abstract workflow (DAX)
   compute_jobs     the number of compute jobs in the executable workflow
   clustered_jobs   the number of clustered jobs in the executable workflow.
   si_tx_jobs       the number of data stage-in jobs in the executable workflow.
   so_tx_jobs       the number of data stage-out jobs in the executable workflow.
   inter_tx_jobs    the number of inter site data transfer jobs in the executable workflow.
   reg_job          the number of registration jobs in the executable workflow.
   cleanup_jobs     the number of cleanup jobs in the executable workflow.
   create_dir_jobs  the number of create directory jobs in the executable workflow.
   dax_jobs         the number of sub workflows corresponding to dax tasks in the executable workflow.
   dag_jobs         the number of sub workflows corresponding to dag tasks in the executable workflow.
   chmod_jobs       the number of jobs that set the xbit of the staged executables
   total_jobs       the total number of jobs in the workflow
   ================ =============================================================================================================

In addition if pegasus-plan encounters an error during the planning
process the metrics message has an additional field in addition to the
fields listed above.

.. table:: Error Message sent by pegasus-plan

   ======== =====================================================================
   JSON KEY DESCRIPTION
   ======== =====================================================================
   error    the error payload is the stack trace of errors caught during planning
   ======== =====================================================================

..

.. note::

   pegasus-plan leaves a copy of the metrics sent in the workflow submit
   directory in the file ending with ".metrics" extension. As a user you
   will always have access to the metrics sent.

.. _usage-pegasuslite-metrics:

Pegasus Lite Metrics
~~~~~~~~~~~~~~~~~~~~

Starting Pegasus 5.1.0 release, PegasusLite wrapped jobs send a location record
that enables us to figure out what resource a job runs on.

The location record can be found toward the end of the job .out file as a pegasus multipart record.

Example below

.. code-block:: yaml

        location:
            geohash: s000
            ip: 10.101.104.66
            latitude: 0
            longitude: 0
            organization: N/A
            subdomain: ads.isi.edu


