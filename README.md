ec2-cost-tools
==============

Simple tools for EC2 reserved instances cost analysis 

Usage
=====

Install the ec2-costs commandline tool by running

```bash
pip install -e .
```

Configure you ssh credentials through

``bash
aws configure --profile <profilename>

```

If you want to see the costs analysis for reserved instances of `us-west-1` region, then run

```bash
ec2-costs us-west-1 -p profile1 -p profile2
```

You will see two tables like this

```
+---------------+------+------------+---------+---------+-------------+----------------+--------------+
| Instance type | VPC  | Zone       | Tenancy | Covered | Instnace ID | Name           | Monthly Cost |
+---------------+------+------------+---------+---------+-------------+----------------+--------------+
| m3.medium     | None | us-west-1b | default | 4 / 4   |             |                |       167.04 |
|               |      |            |         | True    | i-6ca7f0a4  | foo-api-dev2   |        41.76 |
|               |      |            |         | True    | i-1b2474d3  | bar-api-prod   |        41.76 |
|               |      |            |         | True    | i-6c2d7da4  | foo-api-prod   |        41.76 |
|               |      |            |         | True    | i-7ca7f0b4  | bar-api-dev2   |        41.76 |
| m3.medium     | None | us-west-1c | default | 2 / 2   |             |                |        83.52 |
|               |      |            |         | True    | i-00f6a3ca  | foo-api-prod   |        41.76 |
|               |      |            |         | True    | i-b20b5f78  | bar-api-prod   |        41.76 |
+---------------+------+------------+---------+---------+-------------+----------------+--------------+
########## Not in-use reserved instances ##########
+---------------+-------+------------+---------+-------+--------------+
| Instance type | VPC   | Zone       | Tenancy | Count | Monthly Cost |
+---------------+-------+------------+---------+-------+--------------+
| m3.medium     | False | us-west-1c | default | 0     |         0.00 |
| m3.medium     | False | us-west-1b | default | 2     |        83.52 |
+---------------+-------+------------+---------+-------+--------------+
########## Summary ##########
EC2 Monthly Costs: 334.08
EC2 Monthly All On Demand Costs: 332.64
Amount you saved by using reserved: -1.44
Percentage you saved by using reserved: % -0.43
```

The first table indicates all running instacnes, and shows that whether they are covered by reserved instances. You should notice that actually reserved instances have no one-to-one relationship between EC2 instances, it only affects the billing. The `Covered` is just for you to understand the reserved instance coverage easily.

To understand how many reserved instances are not in use, you can see the second table. In our example, there are two reserved instances with m3.medium type, non-VPC in us-west-1b zone are not used.

With these two tables, you can understand how many reserved instances are in-use, then decide how many more to buy or sell.
