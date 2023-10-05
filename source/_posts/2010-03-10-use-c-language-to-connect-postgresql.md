---
layout: post
title: "use c language to connect postgreSQL"
date: 2010-3-10
wordpress_id: 104
comments: true
tags: ["c", "code", "connect", "database", "database", "postgresql", "postgresql", "programming", "remote"]
categories:
- code
- program
---
<meta name="_edit_last" content="1" />
<meta name="views" content="2275" />
There is a small example copied from <a href="http://www.postgresql.org/files/documentation/books/aw_pgsql/writing_apps/node3.html">postgreSQL.org</a>

[codesyntax lang="c" tab_width="4" strict="yes"]
{% codeblock %}/*
*  libpq sample program
*/

#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include "libpq-fe.h"                                   /* libpq header file */

int main()
{
	char        state_code[3];                          /* holds user state code */
	char        query_string[256];                      /* holds constructed SQL query */
	PGconn     *conn;                                   /* holds database connection */
	PGresult   *res;                                    /* holds query result */
	int         i;

	conn = PQconnectdb("dbname=test"); /* connect to the database */

	if (PQstatus(conn) == CONNECTION_BAD)               /* did the connection fail? */
	{
		fprintf(stderr, "Connection to database failed.\n");
		fprintf(stderr, "%s", PQerrorMessage(conn));
		exit(1);
	}

	printf("Enter a state code:  ");                    /* prompt user for a state code */
	scanf("%2s", state_code);

	sprintf(query_string,                               /* create an SQL query string */
	"SELECT name \
	FROM statename \
	WHERE code = '%s'", state_code);

	res = PQexec(conn, query_string); /* send the query */

	if (PQresultStatus(res) != PGRES_TUPLES_OK)         /* did the query fail? */
	{
		fprintf(stderr, "SELECT query failed.\n");
		PQclear(res);
		PQfinish(conn);
		exit(1);
	}

	for (i = 0; i &lt; PQntuples(res); i++)                /* loop through all rows returned */
		printf("%s\n", PQgetvalue(res, i, 0));          /* print the value returned */

	PQclear(res); /* free result */

	PQfinish(conn); /* disconnect from the database */

	return 0;
}{% endcodeblock %}


above is how to connect localhost database. If you want to connect with remote postgreSQL server. You will following those rules(copied from <a href="http://www.cyberciti.biz/tips/postgres-allow-remote-access-tcp-connection.html">vivek</a>):
## Step # 1: Login over ssh if server is outside your IDC
{% codeblock %}$ ssh user@remote.pgsql.server.com{% endcodeblock %}
## Step # 2: Enable client authentication
Once connected, you need edit the PostgreSQL configuration file, edit the PostgreSQL configuration file <strong>/var/lib/pgsql/data/pg_hba.conf</strong> (or <strong>/etc/postgresql/8.2/main/pg_hba.conf</strong> for latest 8.2 version) using a text editor such as vi.

Login as postgres user using su / sudo command, enter:
{% codeblock %}$ su - postgres{% endcodeblock %}
Edit the file:
{% codeblock %}$ vi /var/lib/pgsql/data/pg_hba.conf{% endcodeblock %}
OR
{% codeblock %}$ vi /etc/postgresql/8.2/main/pg_hba.conf{% endcodeblock %}
Append the following configuration lines to give access to 10.10.29.0/24 network:
{% codeblock %}host all all 10.10.29.0/24 trust{% endcodeblock %}
Save and close the file. Make sure you replace 10.10.29.0/24 with actual network IP address range of the clients system in your own network.
## Step # 2: Enable networking for PostgreSQL
You need to enable TCP / IP networking. Use either step #3 or #3a as per your PostgreSQL database server version.
## Step # 3: Allow TCP/IP socket
If you are using <strong>PostgreSQL version 8.x or newer</strong> use the following instructions or skip to <a href="http://www.cyberciti.biz/tips/postgres-allow-remote-access-tcp-connection.html#3a">Step # 3a</a> for older version (7.x or older).

You need to open PostgreSQL configuration file /var/lib/pgsql/data/postgresql.conf or /etc/postgresql/8.2/main/postgresql.conf.
{% codeblock %}# vi /etc/postgresql/8.2/main/postgresql.conf{% endcodeblock %}
OR
{% codeblock %}# vi /var/lib/pgsql/data/postgresql.conf {% endcodeblock %}
Find configuration line that read as follows:
{% codeblock %}listen_addresses='localhost'{% endcodeblock %}
Next set IP address(es) to listen on; you can use comma-separated list of addresses; defaults to 'localhost', and '*' is all ip address:
{% codeblock %}listen_addresses='*'{% endcodeblock %}
Or just bind to 202.54.1.2 and 202.54.1.3 IP address
{% codeblock %}listen_addresses='202.54.1.2 202.54.1.3'{% endcodeblock %}
Save and close the file. Skip to <a href="http://www.cyberciti.biz/tips/postgres-allow-remote-access-tcp-connection.html#4">step # 4</a>.

<a name="3a"></a>
### Step #3a - Information for old version 7.x  or older
Following configuration only required for <strong>PostgreSQL version 7.x or older</strong>. Open config file, enter:
{% codeblock %}# vi /var/lib/pgsql/data/postgresql.conf {% endcodeblock %}
Bind and open TCP/IP port by setting tcpip_socket to true.  Set / modify tcpip_socket to true:
{% codeblock %}tcpip_socket = true{% endcodeblock %}
Save and close the file.

<a name="4"></a>
## Step # 4: Restart PostgreSQL Server
Type the following command:
{% codeblock %}# /etc/init.d/postgresql restart{% endcodeblock %}
## Step # 5: Iptables firewall rules
Make sure iptables is not blocking communication, <a href="http://www.cyberciti.biz/tips/howto-iptables-postgresql-open-port.html">open port 5432</a> (append rules to your iptables scripts or file <a href="http://www.cyberciti.biz/faq/howto-block-ipaddress-of-spammers-with-firewall/">/etc/sysconfig/iptables</a>):
{% codeblock %}iptables -A INPUT -p tcp -s 0/0 --sport 1024:65535 -d 10.10.29.50  --dport 5432\
 -m state --state NEW,ESTABLISHED -j ACCEPT
iptables -A OUTPUT -p tcp -s 10.10.29.50 --sport 5432 -d 0/0 --dport \
1024:65535 -m state --state ESTABLISHED -j ACCEPT{% endcodeblock %}
Restart firewall:
{% codeblock %}# /etc/init.d/iptables restart{% endcodeblock %}
## Step # 6: Test your setup
Use psql command from client system. Connect to remote server using IP address 10.10.29.50 and login using vivek username and sales database, enter:
{% codeblock %}$ psql -h 10.10.29.50 -U vivek -d sales{% endcodeblock %}
## Step #7: Change your code

{% codeblock %}const char * connstr = "host='10.10.29.50' dbname='my_database' user='postgres' password='secret'"
conn = PQconnectdb(connstr);                  /* connect to the database */{% endcodeblock %}
 
