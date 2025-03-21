Some use cases
==============

As a *framework*, IVRE has several possible use cases. Of course, you
probably want to use only parts of what IVRE can do.

Your own Shodan / ZoomEye / Censys / Binaryedgeio / whatever
------------------------------------------------------------

You can use IVRE as a private (or even public, if you want)
alternative to Shodan (or any other similar service).

The main difference with public services is that you will have the
control of your data. You can scan whatever you want (your private
networks, public networks, a specific country or Autonomous System,
the whole Internet, etc.), for any port or protocol. You can run any
query on your data; no-one has to know what you are really looking
for.

Of course, this require more work than just using an existing public
service, but the benefits are huge!

IVRE does not come with a scanner, and takes advantage of `Nmap
<https://nmap.org/>`_, `Masscan
<https://github.com/robertdavidgraham/masscan>`_ and `Zgrab / Zgrab2
<https://zmap.io/>`_. Depending on your use case, you can choose one
or use both (IVRE will happily merge the results for you). Remember to
use the ``-oX`` option (which works with both Nmap and Masscan) or
``-o`` for Zgrab2, as IVRE needs the XML output file for Nmap and
Masscan, and JSON for Zgrab2.

You can use ``ivre runscans``, ``ivre runscansagent`` or
``ivre runscansagentdb`` to run Nmap scans against wide targets (more)
easily.

You will then store the results from the XML or JSON output files into
IVRE database using ``ivre scan2db``.

Finally, use ``ivre db2view nmap`` to create a ``view`` (see
:ref:`overview/principles:Purposes`) that you can explore with the
:ref:`usage/web-ui:Web User Interface`.

See :ref:`usage/kibana:IVRE with Kibana` if you want to use Kibana to
explore your scan results.

Your own Passive DNS service
----------------------------

Passive DNS services log DNS answers into a database and let you run
queries against them.

IVRE uses its `Zeek <https://www.zeek.org/>`_ script ``passiverecon``
to, among others, log DNS answers. They are stored in the ``passive``
purpose (see :ref:`overview/principles:Purposes`) via ``ivre
passiverecon2db`` CLI tool as ``DNS_ANSWER`` records.

They can be queried using ``ivre iphost`` CLI tool, as in the
following example (the results come from a PCAP file used in IVRE's
:ref:`dev/tests:Tests`):

::

   $ ivre iphost ipv4.icanhazip.com
   ipv4.icanhazip.com A 216.69.252.101 (109.0.66.10:53, 1 time, 2014-01-02 09:37:57.197000 - 2014-01-02 09:37:57.197000)
   ipv4.icanhazip.com A 216.69.252.100 (109.0.66.10:53, 1 time, 2014-01-02 09:37:57.197000 - 2014-01-02 09:37:57.197000)
   ipv4.icanhazip.com A 216.69.252.100 (109.0.66.20:53, 1 time, 2014-01-02 09:37:57.197000 - 2014-01-02 09:37:57.197000)
   ipv4.icanhazip.com A 216.69.252.101 (109.0.66.20:53, 1 time, 2014-01-02 09:37:57.197000 - 2014-01-02 09:37:57.197000)

   $ ivre iphost 216.69.252.101
   ipv4.icanhazip.com A 216.69.252.101 (109.0.66.10:53, 1 time, 2014-01-02 09:37:57.197000 - 2014-01-02 09:37:57.197000)
   ipv4.icanhazip.com A 216.69.252.101 (109.0.66.20:53, 1 time, 2014-01-02 09:37:57.197000 - 2014-01-02 09:37:57.197000)

To see an interactive session of IVRE using passive data (including
DNS answers), have a look at :ref:`overview/screenshots:Passive
network analysis`.

YETI plugin
-----------

`Yeti <https://yeti-platform.github.io/>`_ is a platform meant to
organize observables, indicators of compromise, TTPs, and knowledge on
threats in a single, unified repository.

It comes with an "analytics" plugin that uses IVRE's data to create
links between IP addresses, hostnames, certificates, etc.

To learn more about this plugin, have a look at `its documentation
<https://github.com/yeti-platform/yeti/tree/master/contrib/analytics/ivre_api>`__.

|yeti_investigation|

Cortex analyzer
---------------

`Cortex <https://thehive-project.org/>`_ is a tool to analyze
observables for SOCs, CSIRTs and security researchers; it integrates
well with TheHive.

It comes with an "Analyzer" that uses IVRE's data to report
intelligence about Autonomous Systems, certificates, domain and host
names, IP addresses, networks, open ports, etc.

To learn more about this analyzer, have a look at `its documentation
<https://github.com/TheHive-Project/Cortex-Analyzers/blob/develop/analyzers/IVRE/README.md>`__.

|cortex_analyzer_template|

OpenCTI connector
-----------------

`OpenCTI <https://www.opencti.io/>`_ is an open-source cyber threat
intelligence (CTI) platform.

It comes with an "internal enrichment connector" that uses IVRE's data
to create links between IP addresses, MAC addresses, hostnames,
certificates, AS numbers and locations.

To learn more about this connector, have a look at `its documentation
<https://github.com/OpenCTI-Platform/connectors/blob/master/internal-enrichment/ivre/README.md>`__.

|opencti_connector_scans|

|opencti_connector_passive|

Obsidian plugin
---------------

`Obsidian <https://obsidian.md/>`_ is a knowledge base and note-taking
application that relies on Markdown files.

A `community plugin <https://github.com/ivre/obsidian-ivre-plugin>`_
exists that uses IVRE's data to create notes based on IVRE's data that
provides context to your notes related to pentest or red team
engagements, bug bounty hunting, cyber threat intelligence, etc.

See the `plugin's README
<https://github.com/ivre/obsidian-ivre-plugin/blob/master/README.md>`_.

|obsidian_graph|

|obsidian_domain|

|obsidian_host|

Blog posts and other resources
------------------------------

The author's blog has some `IVRE-related blog posts
<http://pierre.droids-corp.org/blog/html/tags/ivre.html>`_ that might be useful.

Here is a list of other blog posts about or around IVRE:

- Scan the hosts that hit your honeypots, and exploit the results!

   - `Who's Attacking Me?
     <https://isc.sans.edu/forums/diary/Whos+Attacking+Me/21933/>`_

   - `Three Honeypots and a Month After
     <https://www.serializing.me/2019/01/27/three-honeypots-and-a-month-after/>`_

- Scanning SAP Services:

   - `gelim/nmap-erpscan <https://github.com/gelim/nmap-erpscan>`_ on
     Github

   - `SAP Services detection via nmap probes
     <https://erpscan.io/press-center/blog/sap-services-detection-via-nmap-probes/>`_

   - `SAP Dispatcher Security <https://erpscan.io/press-center/blog/sap-dispatcher-security/>`_

- `Re-discover your company network with Ivre
  <https://blog.cybsec.xyz/re-discover-your-company-network-with-ivre/>`_

- IVRE tests & reviews:

   - `IVRE <https://security-bits.de/posts/2018/12/07/ivre.html>`_

   - `IVRE! Drunk Frenchman Port Scanner Framework!
     <https://mstajbakhsh.ir/ivre-drunk-frenchman-port-scanner-framework/>`_

   - `Visualizing Scans Part 1: IVRE
     <https://bestestredteam.com/2019/02/10/visualizing-scans-part-1-ivre/>`_

- Spanish:

   - `Reconocimiento de redes con IVRE
     <https://www.welivesecurity.com/la-es/2015/08/11/reconocimiento-de-redes-con-ivre/>`_

You have found (or written) a document that might help other use IVRE
or decide if they need it? Please let us know: `open an issue
<https://github.com/ivre/ivre/issues/new>`_ or :ref:`index:Contact` us
so that we can add a link here!

.. |yeti_investigation| image:: ../screenshots/yeti_investigation.png
.. |cortex_analyzer_template| image:: ../screenshots/cortex-analyzer-template.png
.. |opencti_connector_scans| image:: ../screenshots/opencti-connector-scans.png
.. |opencti_connector_passive| image:: ../screenshots/opencti-connector-passive.png
.. |obsidian_graph| image:: ../screenshots/obsidian_graph_thunderbird.png
.. |obsidian_domain| image:: ../screenshots/obsidian_domain_1password.png
.. |obsidian_host| image:: ../screenshots/obsidian_address_1password.png
