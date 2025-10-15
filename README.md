# DocuSign Addon

## Service Route
**Sending documents for signing**:  
  `/docusignintegration/import/docusign`  
**Sending signed documents back to Workspace workflows**:  
  `/docusignintegration/import/envelopecompleted`  
**Authorizing the integration service to send out envelopes for signing in the name of a user**:  
  `/docusignintegration/consent`

## Description
The **DocuSign Addon** is an integration service for the Itiner Workspace platform that facilitates digital document signing through **DocuSign**. It allows workflows to automatically generate and send signing envelopes to one or more recipients. Once all designated signers complete the signing process, the signed document is returned to the addon, which then attaches it to the originating workflow and updates the `digitalSignResult` workflow variable to indicate completion.

This integration supports multiple simultaneous signers. All signer email addresses must be predefined in the workflow through dedicated variables. Additionally, the service supports:
- **Tagging**: The signed document can be tagged using a value from the `digitalSignAttachmentTag` variable.
- **Custom Email Messages**: Users can specify a message that will appear in the email sent to the signers using the `digitalSignEmailBody` variable.
- **Anchor Tagging**: The addon supports DocuSign's Anchor Tagging functionality. When enabled, DocuSign scans the uploaded document for predefined anchor strings and places signature fields at those locations. In this integration, each signer's anchor string is the name of the corresponding workflow variable used to define their full name (e.g., `digitalSignerName1`, `digitalSignerName2`, etc.). This ensures that the signature field appears exactly where needed without manual placement.

The addon supports **Simple (SES)**, **Advanced (AES)**, and **Qualified (QES)** electronic signatures. Signature type is defined using the `digitalSignType` variable. For AES and QES, the name of the recipient must be provided, othervise the signing envelope cannot be created. For more information regarding different types of digital signature, visit [Types of Digital Signature: AES, QES, SES, explained](https://www.docusign.com/en-gb/blog/types-digital-signature-aes-qes-ses-explained).

> 💡 **Asynchronous Operation**: This integration service operates asynchronously. It sends signing envelopes to DocuSign and relies on webhook callbacks to update the workflow once the envelope status changes. The `digitalSignResult` variable is updated and signed documents are attached only after receiving the 'envelope-completed' event from DocuSign.

### Signing Workflow:
1. The user uploads a document, and sends it to the DocuSign integration addon via an Export task, along with the signer data.
2. The addon creates an envelope and sends it to DocuSign.
3. DocuSign distributes the envelope to all signers **simultaneously**.
4. Once all signers have signed the document:
   - DocuSign returns the signed document to the addon.
   - The signed document is attached to the original workflow.
   - If the `digitalSignAttachmentTag` variable is set, its value is applied as a tag to the attached signed document.
   - The `digitalSignResult` variable is updated to reflect the envelope status (e.g., `"envelope-completed"`, `"envelope-deleted"`).

You can find the full list of possible envelope event types in the [DocuSign Connect Event Triggers documentation](https://developers.docusign.com/platform/webhooks/connect/event-triggers/).

## Dedicated Template Variables
- **digitalSignerEmail[1-9]**: Email addresses of the signers (e.g., `digitalSignerEmail1`, `digitalSignerEmail2`, etc.). If there is only one signer then `digitalSignerEmail` can be used as well, without the numbering.
- **digitalSignerName[1-9]**: Full name(s) of the signers. If there is only one signer then `digitalSignerName` can be used as well, without the numbering.
- **digitalSignType**: Defines the required level of digital signature. Accepts `SES` (Simple Electronic Signature), `AES` (Advanced Electronic Signature), or `QES` (Qualified Electronic Signature). If no value is given, then the default value is `SES`.
- **digitalSignResult**: A string variable updated to reflect the current status of the envelope as reported by DocuSign (e.g., `"envelope-completed"`, `"envelope-deleted"`).
- **digitalSignAttachmentTag**: The value of this variable is added as a tag to the signed document when it is attached to the workflow.
- **digitalSignEmailBody**: The custom message that will appear in the email sent to signers via DocuSign.

> When using multiple signers, the addon matches each `digitalSignerEmail[n]` with its corresponding `digitalSignerName[n]` to ensure correct assignment.

---

This service is part of the **Itiner Workspace integrations**, enabling secure and automated digital signature workflows by leveraging the capabilities of DocuSign.
