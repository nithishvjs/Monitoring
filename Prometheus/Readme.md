Store the service files on /etc/systemd/system/
(both prometheus.service and node_exporter.service)

After saved the service files, Run

systemctl daemon-reload
systemctl start prometheus.service
systemctl enable prometheus.service

In prometheus.yml file,

[- job_name: "ec2_sd"
  ec2_sd_configs:
    - region: us-east-1
      port: 9100
      filters:
        - name: tag:Monitoring
          values:
            - true
  relabel_configs:
    - source_labels: [__meta_ec2_instance_id]
      target_label: instance_id
    - source_labels: [__meta_ec2_private_ip]
      target_label: private_ip
    - source_labels: [__meta_ec2_tag_Name]
      target_label: name ]

It is used when an EC2 is created with a tag tag:Monitoring and download node-exporter on that machine, It will automatically added on prometheus target.
