Digital forensics and incident response (DFIR)
===========================================================

Memory forensics ties into many disciplines in cyber investigations. From the classical investigations that focus on user artifacts via malware analysis to large-scale hunting, memory forensics has many applications in security operations.

Though DFIR is traditionally a reactive security function, tooling and advanced technology such as artificial intelligence (AI) and machine learning (ML), have enabled some organisations to leverage DFIR activity to influence and inform preventative measures. In such cases, making it a component within a proactive security strategy.

.. image:: _static/images/in-progress.png
  :alt: Forever in progress ...

----

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: Testlab

   Ventoy <https://testlab.tymyrddin.dev/docs/drives/ventoy>
   Kali <https://testlab.tymyrddin.dev/docs/vm/kali>
   CAINE <https://testlab.tymyrddin.dev/docs/vm/caine>
   SquashFS <https://testlab.tymyrddin.dev/docs/dfir/squashfs>
   DFIR tools <https://testlab.tymyrddin.dev/docs/dfir/README>
   Mobile tools <https://testlab.tymyrddin.dev/docs/mobile/readme>

----

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Notes

   docs/notes/README.md
   docs/notes/choreography.md
   docs/notes/preparation.md
   docs/notes/acquisition.md
   docs/notes/access.md
   docs/notes/windows.md
   docs/notes/linux.md
   docs/notes/macos.md
   docs/notes/mobile.md
   docs/notes/ios.md
   docs/notes/android.md
   docs/notes/network.md
   docs/notes/resources.md

----

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: Root-me challenges

   docs/root-me/README.md
   docs/root-me/c&c2.md
   docs/root-me/docker-layers.md
   docs/root-me/log-web-attack.md
   docs/root-me/c&c5.md
   docs/root-me/cat.md
   docs/root-me/rubber-duck.md
   docs/root-me/c&c3.md
   docs/root-me/vault.md
   docs/root-me/c&c4.md
   docs/root-me/interview1.md
   docs/root-me/c&c6.md
   docs/root-me/interview2.md

--------

.. toctree::
   :glob:
   :maxdepth: 1
   :includehidden:
   :caption: TryHackMe rooms

   docs/thm/README.md
   docs/thm/server.md
   docs/thm/desktop.md
   docs/thm/standard.md
   docs/thm/ioc-collector.md
   docs/thm/ioc-analysis.md
   docs/thm/endpoint.md
   docs/thm/leaky.md
   docs/thm/windows10.md
   docs/thm/policy.md
   docs/thm/bob.md
   docs/thm/feelings.md
   docs/thm/nightmare.md

----

.. toctree::
   :maxdepth: 1
   :includehidden:
   :caption: More practice

   DFRWS Forensic challenges <https://dfrws.org/forensic-challenges/>
   HN/P challenges <https://www.honeynet.org/challenges/>
   Malware traffic analysis exercises <https://www.malware-traffic-analysis.net/training-exercises.html>
   Geoguessr (Geolocation game) <https://www.geoguessr.com/>

----

Books
---------------------------

.. grid:: 3
    :gutter: 1

    .. grid-item-card::
        :link: https://www.packtpub.com/product/digital-forensics-and-incident-response-third-edition/9781803238678

        .. image:: _static/images/bookcovers/digital-forensics-incident-response.png

    .. grid-item-card::
        :link: https://www.packtpub.com/product/incident-response-in-the-age-of-cloud/9781800569218

        .. image:: _static/images/bookcovers/ir-in-age-of-cloud.png

    .. grid-item-card::
        :link: https://www.packtpub.com/product/learn-computer-forensics/9781838648176

        .. image:: _static/images/bookcovers/learn-computer-forensics.png

.. grid:: 3
    :gutter: 1

    .. grid-item-card::
        :link: https://nostarch.com/practical-linux-forensics

        .. image:: _static/images/bookcovers/practical-linux-forensics.png

    .. grid-item-card::
        :link: https://nostarch.com/forensicimaging/

        .. image:: _static/images/bookcovers/practical-forensic-imaging.png

    .. grid-item-card::
        :link: https://www.packtpub.com/product/practical-mobile-forensics-third-edition/9781788839198

        .. image:: _static/images/bookcovers/practical-mobile-forensics.png
