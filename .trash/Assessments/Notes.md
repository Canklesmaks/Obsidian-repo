# general

![[Pasted image 20240305140318.png]]
                                             ^and
![[Pasted image 20240305140523.png]]

# Vulnerability assessment

#### Key terms
* `Vulnerability` is a weakness or bug in an organization's environment, including applications, networks, and infrastructure, that opens up the possibility of threats from external actors
* `Threat` is a process that amplifies the potential of an adverse event, such as a threat actor exploiting a vulnerability. Some vulnerabilities raise more threat concerns over others due to the probability of the vulnerability being exploited.
* `Exploit` is any code or resources that can be used to take advantage of an asset's weakness.
* `Risk` is the possibility of assets or data being harmed or destroyed by threat actors.

# Assessment Standards

* The [Payment Card Industry Data Security Standard (PCI DSS)](https://www.pcisecuritystandards.org/pci_security/) is a commonly known standard in information security that implements requirements for organizations that handle credit cards.
* `HIPAA` is the [Health Insurance Portability and Accountability Act](https://www.hipaa.com/), which is used to protect patients' data. HIPAA does not necessarily require vulnerability scans or assessments; however, a risk assessment and vulnerability identification are required to maintain HIPAA accreditation.
* `ISO 27001` is a standard used worldwide to manage information security. [ISO 27001](https://www.iso.org/isoiec-27001-information-security.html) requires organizations to perform quarterly external and internal scans.
* The `International Organization for Standardization` (`ISO`) maintains technical standards for pretty much anything you can imagine. The [ISO 27001](https://www.iso.org/isoiec-27001-information-security.html) standard deals with information security. ISO 27001 compliance depends upon maintaining an effective Information Security Management System. To ensure compliance, organizations must perform penetration tests in a carefully designed way.

# Penetration Testing Standards

#### PTES

The [Penetration Testing Execution Standard](http://www.pentest-standard.org/index.php/Main_Page) (`PTES`) can be applied to all types of penetration tests. It outlines the phases of a penetration test and how they should be conducted. These are the sections in the PTES:

- Pre-engagement Interactions
- Intelligence Gathering
- Threat Modeling
- Vulnerability Analysis
- Exploitation
- Post Exploitation
- Reporting

#### OSSTMM

`OSSTMM` is the `Open Source Security Testing Methodology Manual`, another set of guidelines pentesters can use to ensure they're doing their jobs properly. It can be used alongside other pentest standards.

[OSSTMM](https://www.isecom.org/OSSTMM.3.pdf) is divided into five different channels for five different areas of pentesting:

1. Human Security (human beings are subject to social engineering exploits)
2. Physical Security
3. Wireless Communications (including but not limited to technologies like WiFi and Bluetooth)
4. Telecommunications
5. Data Networks
#### NIST

The `NIST` (`National Institute of Standards and Technology`) is well known for their [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework), a system for designing incident response policies and procedures. NIST also has a Penetration Testing Framework. The phases of the NIST framework include:

- Planning
- Discovery
- Attack
- Reporting

#### OWASP

`OWASP` stands for the [Open Web Application Security Project](https://owasp.org). They're typically the go-to organization for defining testing standards and classifying risks to web applications.

OWASP maintains a few different standards and helpful guides for assessment various technologies:

- [Web Security Testing Guide (WSTG)](https://owasp.org/www-project-web-security-testing-guide/)
- [Mobile Security Testing Guide (MSTG)](https://owasp.org/www-project-mobile-security-testing-guide/)
- [Firmware Security Testing Methodology](https://github.com/scriptingxss/owasp-fstm)

## Common Vulnerability Scoring System (CVSS)
The CVSS is often used together with the so-called [Microsoft DREAD](https://en.wikipedia.org/wiki/DREAD_(risk_assessment_model)). `DREAD` is a risk assessment system developed by Microsoft to help IT security professionals evaluate the severity of security threats and vulnerabilities. It is used to perform a risk analysis by using a scale of 10 points to assess the severity of security threats and vulnerabilities. With this, we calculate the risk of a threat or vulnerability based on five main factors:

- Damage Potential
- Reproducibility
- Exploitability
- Affected Users
- Discoverability
## Common Vulnerabilities and Exposures (CVE)
* [Open Vulnerability Assessment Language (OVAL)](https://oval.mitre.org/) is a publicly available information security international standard used to evaluate and detail the system's current state and issues.

	The four main classes of OVAL definitions consist of:

- `OVAL Vulnerability Definitions`: Identifies system vulnerabilities
- `OVAL Compliance Definitions`: Identifies if current system configurations meet system policy requirements
- `OVAL Inventory Definitions`: Evaluates a system to see if a specific software is present
- `OVAL Patch Definitions`: Identifies if a system has the appropriate patch

* [Common Vulnerabilities and Exposures (CVE)](https://cve.mitre.org/) is a publicly available catalog of security issues sponsored by the United States Department of Homeland Security (DHS). Each security issue has a unique CVE ID number assigned by the CVE Numbering Authority (CNA). The purpose of creating a unique CVE ID number is to create a standardization for a vulnerability or exposure as a researcher identifies it.

## Stages of Obtaining a CVE
#### Stage 1: Identify if CVE is Required and Relevant
* negative impact to confidentiality, integrity, OR availability
#### Stage 2: Reach Out to Affected Product Vendor
* A researcher should ensure they have made a good faith effort to contact a vendor directly.
#### Stage 3: Identify if Request Should Be For Vendor CNA or Third Party CNA

#### Stage 4: Requesting CVE ID Through CVE Web Form

#### Stage 5: Confirmation of CVE Form

#### Stage 6: Receival of CVE ID

#### Stage 7: Public Disclosure of CVE ID 
* CVE IDs can be announced to the public as soon as appropriate vendors and parties are aware of the issue to prevent duplication of CVE IDs. This stage ensures that all associated parties are aware of the problem before being publicly disclosed.
#### Stage 8: Announcing the CVE

#### Stage 9: Providing Information to The CVE Team
