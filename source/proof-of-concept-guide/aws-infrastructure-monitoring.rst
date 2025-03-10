.. Copyright (C) 2015, Wazuh, Inc.

.. meta::
  :description: This PoC shows how the Wazuh module for AWS (aws-s3) enables log data gathering from different AWS sources. Learn more about it in our documentation.

.. _poc_aws_monitoring:

Amazon AWS infrastructure monitoring
====================================

This PoC shows how the Wazuh module for AWS (aws-s3) enables log data gathering from different AWS sources.

To learn more about monitoring AWS resources, see the :doc:`Using Wazuh to monitor AWS </amazon/index>` section of the documentation.


Configuration
-------------

Configure your environment as follows to test the PoC.

#. Enable ``aws-s3`` wodle in the ``/var/ossec/etc/ossec.conf`` configuration file at the Wazuh manager.

   .. code-block:: XML

      <wodle name="aws-s3">
        <disabled>no</disabled>
        <remove_from_bucket>no</remove_from_bucket>
        <interval>30m</interval>
        <run_on_start>yes</run_on_start>
        <skip_on_error>no</skip_on_error>
        <bucket type="cloudtrail">
            <name>${replace_by_your_cloudtrail_bucket_name}</name>
            <access_key>${replace_by_your_AwsAccessKey}</access_key>
            <secret_key>${replace_by_your_AwsSecretKey}</secret_key>
            <only_logs_after>2021-AUG-01</only_logs_after>
        </bucket>
        <bucket type="guardduty">
            <name>${replace_by_your_guarduty_bucket_name}</name>
            <path>guardduty</path>
            <access_key>${replace_by_your_AwsAccessKey}</access_key>
            <secret_key>${replace_by_your_AwsSecretKey}</secret_key>
            <only_logs_after>2021-AUG-01</only_logs_after>
        </bucket>
        <bucket type="custom">
            <name>${replace_by_your_bucket_name}</name>
            <path>macie</path>
            <access_key>${replace_by_your_AwsAccessKey}</access_key>
            <secret_key>${replace_by_your_AwsSecretKey}</secret_key>
            <only_logs_after>2021-AUG-01</only_logs_after>
        </bucket>
        <bucket type="vpcflow">
            <name>${replace_by_your_bucket_name}</name>
            <path>vpc</path>
            <access_key>${replace_by_your_AwsAccessKey}</access_key>
            <secret_key>${replace_by_your_AwsSecretKey}</secret_key>
            <only_logs_after>2021-AUG-01</only_logs_after>
        </bucket>
        <service type="inspector">
            <access_key>${replace_by_your_AwsAccessKey}</access_key>
            <secret_key>${replace_by_your_AwsSecretKey}</secret_key>
        </service>
      </wodle>

#. Restart the Wazuh manager to apply the changes.

    .. code-block:: console

        # systemctl restart wazuh-manager

Steps to generate the alerts
----------------------------

No action is required. Alerts are automatically generated from AWS logs when using out-of-the-box rules; they appear as soon as they are fetched from the AWS S3 bucket.

Query the alerts
----------------

You can visualize the alert data in the Wazuh dashboard. To do this, go to the **Security events** module and add the filters in the search bar to query the alerts.

- ``rule.groups: "amazon"``

.. thumbnail:: ../images/poc/Amazon-AWS-infrastructure-monitoring.png
          :title: Amazon AWS infrastructure monitoring
          :align: center
          :wrap_image: No
