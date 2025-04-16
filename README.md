The **DocuSign Addon** is an integration service for the Itiner Workspace platform that facilitates digital document signing through **DocuSign**. It allows workflows to automatically generate and send signing envelopes to one or more recipients. Once all designated signers complete the signing process, the signed document is returned to the addon, which then attaches it to the originating workflow and updates a workflow variable to indicate completion.

This integration supports multiple simultaneous signers. All signer email addresses must be predefined in the workflow through dedicated variables. Additionally, the service supports tagging of the signed document using a workflow variable. When the signed document is returned, the value of the `digitalSignAttachmentTag` variable will be applied as a tag to the attached file.

The service ensures seamless, automated management of the digital signing lifecycle within Itiner Workspace.
