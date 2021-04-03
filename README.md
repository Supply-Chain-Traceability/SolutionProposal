# SCM Architecture 

The following is the proposed architecture for supply chain management
using the required artifacts: SCA/Product certificates, the PKI
infrastructure, the Ethereum SmartContract and the Certificate Storage
solution (WallID Smart Contract, Storage and User Access API).

The architecture is described in terms of:

1.  The proposed SCM functions and Use cases

2.  The proposed Ethereum Smart Contract (that implements the SCM
    functions and use cases)

3.  The WallID Import/Validate certificate architecture

4.  The defined Public Key Infrastructure

5.  The proposed PKI and certificate Generation, Import and Validation

# Proposed SCM functions and Use Cases

##  {#section .list-paragraph}

## SCM Functions

**A. Register SCA**

Any authorized SCA can register itself with the Supply Chain SC. To
register a SCA must provide proof of its identity that has been made
with the governmental or supply chain organizational entities. The
objective is to provide decentralized authentication and avoid
impersonation of the SCA. The SCA ID certificates are stored via
function H. and validated via function I. (reusing WalliD architecture).

**B. Register Product**

Any previously authorized SCA can register a product token with the
Supply Chain Smart Contract. The objective is to allow for the minimum
set of attributes to support traceability stored in BC (SC storage). The
selected product attributes are the EPC, quantity, geolocation,
ownership, custody state and if a certificate was provided. The addition
of a product certificate is optional so the supply chain could if
required operate with no product certificates but still provide product
traceability with validated SCAs. The product certificate is imported in
a storage provider (a specific role in the WalliD architecture) in the
same way as for the SCA certificate and that SCA will remain the product
certificate owner.

**C. Get/Set Product Attributes**

For an owned product it is possible to get/set some of the product token
attributes namely: ownership, location and custody state.

**D. Get Product Certificate**

The current SCA product owner can request the SCA product certificate
owner to retrieve the certificate from StoreId Provider and provide it
to the Certificate Validator (SCM Manager). The certificate will then be
available in the Certificate Validator website for the current SCA Owner
to view. It shall also be possible for the consumer to use the SCM
Manager as proxy to request to view the product certificate. This is the
only SCM functionality that is provided to non SCAs (via SCM Manager
proxy).

**E. Transform Product**

A SCA with transformation role can transform an existing product, copy
the existing product attributes to a new product (new EPC) thus updating
the inventory of products.

**F. Transfer ownership to another SCA**

A SCA with ownership of a product can tentatively change ownership to a
SCA (destination address is set), waiting for other SCA to commit
(logistics handoff).

**G. Receive ownership from**

The previous process is only complete when the receiver SCA calls
"Transfer ownership from" and the sender and receiver BC address are
verified.

**H. Import ID and Certificates**

Operates over WalliD SC and architecture - these functions operate using
events that are sent by the SC in 2 occasions: when a SCA registers and
when a product certificate is added. Both events initiate a sequence of
actions from the SC to the SCA that has the certificate (see Figure 14
-- Import certificate architecture in Appendix 5 - Certificate import
and validation) and ends when the certificates are stored in "Store
Provider" and validated in SC by "Certificate Validator".

**I. Provide ID and Certificates for validation**

Operates over WalliD SC and architecture - these functions operate using
the events "perform KYC" /" perform KYP" after the previous import. KYC
(Know Your Customer) / KYP (Know Your Product) are similar use cases
where the identity is verified either for user or for product. These
flows end when "Certificate Validator" validates either the SCA or EPC
(Electronic Product Code).

**J. Validate Certificates**

The SCM "Certificate Validator" must perform the hashing algorithm and
verify the provided ID and certificate information and chain of trust
are valid and not revoked to make proof to the Supply Chain SC that the
registered SCA has a true identity and thus can operate on the supply
chain. This also applies to a product so that the product identity
provided is certified and its certificate is available to be viewed.

K. View EPC Certificate

SCAs and customers can use the Certificate Validator website address and
call Get Product Certificates (EPC). In the case of customers, they use
"Certificate Validator" as a proxy to access and retrieve the product
certificate. This function is only available to "Certificate Validator"
when the product is in custody state: "sold" These functionalities are
implemented in the SC artifact. For details on the SC code see Appendix
6 - Ethereum Smart Contract and for details on interworking with WalliD
architecture see Appendix 5 - Certificate import and validation. For
details on Public Key Infrastructure (PKI) see Appendix 7 - Public Key
Infrastructure and for the establishment of the PKI using OpenSSL[^1]
certificates see Appendix 10 - PKI setup.

## Proposed SCM Use cases

**Use case A: Add a new SCA to the supply chain management system.**

A SCA must firstly authenticate himself before being able to interact
with the SCM. For this he must register with a trusted entity that makes
sure he has the correct credentials and authorization to interact with
the SCM. Following the requirement of having a decentralized system and
in accordance with the selected use case (PDO alimentary supply chain)
the trusted entities were considered to be the
Governmental/Organizational agencies. In the case of a centralized SCM
(e.g., over private/consortium BC) another central entity could be
chosen, and the certificate validator could also become the certificate
issuer (CA).

![](/Media/image1.png){width="6.506944444444445in"
height="2.4270833333333335in"}

*Figure 4. Use case A: Add a new SCA*

Use case B1: Add a new product to supply chain

![](/Media/image2.png){width="6.506944444444445in"
height="1.6131944444444444in"}

*Figure 5. Use case B1: Add a new product*

Use case B2: request certificate for product

![](/Media/image3.png){width="6.508333333333334in"
height="1.7284722222222222in"}

*Figure 6. Use case B2: Add a new product*

Use case B3-- Register a new product in SCM

![](/Media/image4.png){width="6.509027777777778in"
height="1.9569444444444444in"}

Figure 7 -- Use case B3 -- Register a new product

Use case C -- Get/Set product attributes

*Figure 7. Use case B3: Register a new product*

Use case C -- Get/Set product attributes

![](/Media/image5.png){width="4.741929133858267in"
height="1.5131944444444445in"}

*Figure 8. Use case C: Get/Set product attributes*

Use case D -- Get product certificate

![](/Media/image6.png){width="6.513888888888889in"
height="1.4201388888888888in"}

*Figure 9. Use case D: Get product certificate*

Use case E-- Transform product

![](/Media/image7.png){width="6.5in" height="1.7590277777777779in"}

*Figure 10. Use case E: Transform product*

Use case F/G-- Transfer ownership to/from

![](/Media/image8.png){width="6.506944444444445in"
height="1.2201388888888889in"}

*Figure 11. Use case F/G: Transfer ownership*

***\
***

# Ethereum Smart Contract

The most updated version and complete SC code is available at:
<https://github.com/prgazevedo/DLT_Masters/tree/master/SCM_SmartContracts>.

This code compiles for solidity version 0.5.11.

Detail on SCA and Validation

![](/Media/image9.png){width="6.893931539807524in"
height="3.3781517935258094in"}

*Figure 16. SCA registration and validation*

Detail on Product registration and transformation

![](/Media/image10.png){width="6.5in" height="2.9972222222222222in"}

*Figure 17. Product registration and transformation*

Detail on transfer of custody and loss of Product

![](/Media/image11.png){width="6.448305993000875in"
height="3.134027777777778in"}

*Figure 18. Transfer of custody*

![](/Media/image12.png){width="6.506944444444445in"
height="2.4368055555555554in"}Detail on Get/Set functions

*Figure 19. Get/Set functionality*

# WalliD architecture

WalliD Import certificate
![](/Media/image13.png){width="6.506944444444445in"
height="3.0069444444444446in"}

*Figure 14. Import certificate architecture*

WalliD Validate certificate

![](/Media/image14.png){width="6.506944444444445in" height="3.19375in"}

*Figure 15. Validate certificate architecture*

SCM Certificate Import and Validation

To import and validate the certificates the SCM solution interacts with
the WalliD architecture for 2 use cases: import of certificates into the
WalliD store provider and afterwards certificate retrieval from the
store provider. These actions are run in sequence with 2 events:
ImportID and following the correct import RequestKYC/RequestKYP. The
same pattern is used for both the registration of SCAs and the
registration of products.

![](/Media/image15.png){width="4.100519466316711in"
height="3.010538057742782in"}

*Figure 12. Certify product*

![](/Media/image16.png){width="4.035332458442695in"
height="3.89924321959755in"}

*Figure 13. Certify SCA*

Defined Public Key Infrastructure and Setup

The main components of a PKI are Public Key Certificates and Certificate
Authorities. PKC - public key certificate, also known as a digital
certificate or identity certificate, is an electronic document used to
prove the ownership of a public key. The certificate includes
information on the public key of the subject, the identity of the
subject and the digital signature of the issuer that has verified the
certificate\'s contents. In a PKI there are 3 main roles and procedures
for a certificate: authenticating the identity carried out by the RA
(Registration Authority), issuance of certificate carried out by the CA
(certification authority) and validation of certificates carried out by
the VA (validation authority. A distrusting 3rd party can trust the
subscriber when the digital signature (PKC) is valid and the 3rd party
trusts the issuer (CA). A certificate binds the public key with the
identity (distinguished name) of an entity (subscriber).

![](/Media/image17.png){width="6.509027777777778in"
height="2.017361111111111in"}

*Figure 20. X.509 certificate*

Registration and certification procedures: A Registration Authority (RA)
receives a request for the digital certificate (CSR) from the subscriber
that needs a certificate. The RA verifies the identity of the user and
the information provided. After verification it triggers the CA to sign
a certificate based on that information using the information provided
by the user and its private key. The certificates and the CA's public
keys are made publicly available.

![](/Media/image18.png){width="4.183438320209974in"
height="2.7647069116360456in"}

*Figure 21. PKI certification procedure*

![](/Media/image19.png){width="3.9471073928258966in"
height="2.7699004811898513in"}

*Figure 22. PKI validation procedure*

The validation step is performed online by the Validation Authority
(VA). It is possible for a Certification Authority (CA) to merge all 3
functionalities.

# Proposed certificate generation, import and validation

PDO Use case example and generation

In the next figure is presented the linkage between the livestock
(bovine) government assigned ID and the PDO organization assigned
certificate ID. These 3 documents presented were collected at the local
supply chain exemplify the main data and attributes that are required to
establish traceability for this use case.

![](/Media/image20.png){width="4.707979002624672in"
height="3.6277066929133857in"}

*Figure 23. Use case certificate*

The government assigned ID of Bovine (SNIRA ID) is attributed at birth
by DGAV and stored in Sistema Nacional de Informação e Registo Animal
(SNIRA) by. At the same time of birth, the genealogy of calf (bull ID
and Cow ID) is recorded by the PDO organization and is also recorded in
(SNIRA) by IFAP[^2]. When the bovine is ready, it is then sent to a
certified slaughterhouse where the registry of both SNIRA ID and
certificate linkage is assured. At this time the carcass is assigned a
EPC code and a physical tag with the ID of the slaughter house (PT-T
18-CE). The carcass is then shipped to the retailers or seller of the
end product that can be either a consumer beef produce (in the case of
butcher or supermarket) or a prepared meal at a restaurant or hotel.
Each of the SCAs receive the PDO paper certificate together with the
invoice on each carcass.

EPC detail

In order to have unique global identification at instance level
granularity a EPC: Electronic Product Code -- GS1 SGTIN (Serialized
GTIN) or SSCC (Serial Shipping Container Code) identifier is required
The next figure presents the different fields in a SGTIN EPC.

![](/Media/image21.png){width="6.506944444444445in"
height="3.173611111111111in"}*Figure 24. EPC structure*

In the case of SGTIN it is composed of a GTIN (Global Trade
Identification Number) plus a serial ID for unique identification of
each product. SSCC is also EPC and is similar to SGTIN but is it is
mostly used for identifying shipping units uniquely, for example a
pallet or handling unit. When EPC codes are transmitted into traditional
centralized supply chain systems it is generally within the framework of
Electronic Product Code Information Services (GS1 EPCIS standard) a
standard used to create and share event data collected along the 4
dimensions: what, when, where, why for trade objects. This data standard
follows the framework: identify (e.g., GS1 EPC), capture (e.g., using
barcodes or RFID) and share (e.g., via SOAP/XML) and is applied
regularly within logistics companies supply chain systems. However as
mentioned by Tröger & Alt (2017) the volume of data that is generated in
single company EPCIS SCM systems although still under the terabyte it is
rising and progressively necessitating cloud and big data. The volume of
data is consequence of the verbosity of the standard (XML) as can be
viewed in the excerpt below.

![](/Media/image22.png){width="4.706944444444445in"
height="2.4138888888888888in"}

*Figure 25. EPCIS XML sample*

It is then clear that the EPCIS data format is not suitable for BC and
this is thus a further reason to use a much more succinct representation
in the tokenization of products as single EPCs in the proposed SCM SC.

PKI setup

In order to provide products certificates a PKI needs to be setup. It is
recommended that an hierarchical PKI structure is setup in order to
improve security (e.g. by having the root CA offline) and distribute
responsibility. The proposed PKI for the use case shall have 3 levels
the root CA, inter/Mediate CA and end user. The root CA needs to be a
most trusted entity, in this case it can be the Portuguese governmental
institution IFAP (Instituto de Financiamento da Agricultura e Pescas)
which is ruled in Portugal by the Agriculture Ministry. The inter/Mediate
CA needs to be a trusted certifier entity that is verified and trusted
by the IFAP, in this case the PDO association and certificer
"Mirandesa". The end user shall be the certificate requester and in the
sample use case is "AgroGranjo" which is the producer/supplier in the
supply chain. To establish the PKI each CA must validate and sign
certificates in a chain of trust as follows. To implement the PKI and
generate the certificates OpenSSL application was used. The OpenSSL
program is a vast library with a big number of commands, each of which
often with many options and arguments. Many commands use an external
configuration file where the user specifies a configuration file. To
establish the PKI, we define the root CA, then the inter/Mediate CA and,
finally, the Producer\'s certificate requests. For CA root establishment
the entity responsible needs to run following commands.

![](/Media/image23.png){width="6.509027777777778in"
height="0.9826388888888888in"}

*Figure 26. Root CA certificate commands*

For inter/Mediate CA establishment we need to run following commands:

![](/Media/image24.png){width="6.509722222222222in"
height="1.5013888888888889in"}

*Figure 27. Inter/Mediate CA certificate commands*

The resulting inter/Mediate.cert.pem will be used to sign the product
certificate after a certificate signing request is sent from the end
user "Agrogranjo".

Product certificate generation

As described in order to univocally associate the PDO certificate with
the product identification the digital certificate should include: a EPC
global identifier, the governmental identifier and the PDO identifier. A
sample EPC global identifier for the use case can be created[^3] to a
Tag URI: urn:epc:tag:sgtin-96: 2.560123.3456001.823310118 or pure URI:
urn:epc:id:sgtin: 560123.3456001.823310118 which is a valid global
product identifier that can be used in any supply chain or EPCIS system.
For the case of the bovine PDO we must add the SNIRA ID: PT823310118 and
the Genealogy ID: EL60A02018005. The validity of the digital certificate
should follow the rules of the physical certificate (e.g., 15 days). To
use X509 extensions (as defined in OpenSSL X509 V3) we use a
configuration file for the CA authority (the Mirandesa organization
issuing PDO certificates on their products). The Producer "Agrogranjo"
generates a certificate request using: open ssl genrsa -aes256 \\ - out
./private/Supplier.key.pem 4096. In order to create the Product CSR it
is practical to use a configuration file which includes the EPC Tag
URI/SNIRA ID/Genealogy ID as follows.

![](/Media/image25.png){width="6.340265748031496in"
height="3.4935050306211726in"}

*Figure 28. CSR configuration file*

Note that to include the product data as a subjectAltName the otherName
format is used. This is defined in RFC4043 that requires extra data
should be prepended with a OID (as defined by GS1 EPCglobal Certificate
Profile Specification[^4] . In the case of SNIRA and PDO IDs a private
sample generated OID was provided via Windows script[^5]. The Producer
"Agrogranjo" can create a CSR as follows:

![](/Media/image26.png){width="6.506944444444445in"
height="2.713888888888889in"}

*Figure 29. Certificate Request with Product data*

Now at the Inter/Mediate CA "Mirandesa" we use following procedure to
issue the certificate.

![](/Media/image27.png){width="6.264635826771654in"
height="3.245952537182852in"}

*Figure 30. Product Certificate*

This valid certificate is now ready to be used in the SCM over BC,
imported to WalliD provider and supplied to the SCM certificate
validator for verification.

Certificate revocation

The complete revocation workflow is as follows:

![](/Media/image28.png){width="6.406104549431321in"
height="3.3609831583552054in"}

*Figure 31 -- Revoke product certificate*

The now revoked certificate is added to the CRL and can be accessed by
any interested party (e.g., the SCM certificate validator).

[^1]: OpenSSL is a general-purpose cryptography library licensed under
    an Apache-style license that is

    used to both secure communications and implement PKE and PKI:
    https://www.openssl.org/

[^2]: More details in
    https://tradicional.dgadr.gov.pt/pt/cat/carne/carne-de-bovino/235-carne-mirandesa-dop

[^3]: EPC converter at http://convert.erfideo.com/Home/

[^4]: Certificate profile specification available at:
    https://bit.ly/2QWsGMx

[^5]: OID generating script available at: https://bit.ly/37VxcB4
