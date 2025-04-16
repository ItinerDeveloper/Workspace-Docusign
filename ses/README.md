# DocuSign SES Addon

## Service Route
**Sending documents for signing**:  
  `/docusignintegration/import/docusign`  
**Sending signed documents back to Workspace workflows**:  
  `/docusignintegration/import/envelopecompleted`  
**Authorizing the integration service to send out envelopes for signing in the name of a user**:  
  `/docusignintegration/consent`

## Description
The **DocuSign SES Addon** is an integration service for the Itiner Workspace platform that facilitates digital document signing through **DocuSign**. It allows workflows to automatically generate and send signing envelopes to one or more recipients. Once all designated signers complete the signing process, the signed document is returned to the addon, which then attaches it to the originating workflow and updates a workflow variable to indicate completion.

This integration supports multiple simultaneous signers. All signer email addresses must be predefined in the workflow through dedicated variables. Additionally, the service supports tagging of the signed document using a workflow variable. When the signed document is returned, the value of the `digitalSignAttachmentTag` variable will be applied as a tag to the attached file.

The service ensures seamless, automated management of the digital signing lifecycle within Itiner Workspace.

### Signing Workflow:
1. The user uploads a document, and sends it to the DocuSign integration addon via an Export task, along with the signer data.
2. The addon creates an envelope and sends it to DocuSign.
3. DocuSign distributes the envelope to all signers **simultaneously**.
4. Once all signers have signed the document:
   - DocuSign returns the signed document to the addon.
   - The signed document is attached to the original workflow.
   - If the `digitalSignAttachmentTag` variable is set, its value is applied as a tag to the attached signed document.
   - The `digitalSignResult` variable is updated to `"true"` in the workflow to indicate completion.

## Dedicated Template Variables
- **digitalSignerEmail[1-9]**: Email addresses of the signers (e.g., `digitalSignerEmail1`, `digitalSignerEmail2`, etc. If there is only one signer then `digitalSignerEmail` can be used as well, without the numbering.). 
- **digitalSignResult**: Boolean-like string variable set to `"true"` when the document is fully signed and returned.
- **digitalSignAttachmentTag**: The value of this variable is added as a tag to the signed document when it is attached to the workflow.

---

This service is part of the **Itiner Workspace integrations**, enabling secure and automated digital signature workflows by leveraging the capabilities of DocuSign.
