# People’s Court for Gemini CLI

**Experimental draft configuration - October 16, 2026.**
People’s Court is provided by Epistemic Labs Inc.
This extension configures Gemini CLI to use the existing hosted MCP service at `https://mcp.peoplescourt.ai/mcp`.
It contains configuration and workflow context, with no executable code, dependencies, embedded credentials, or new service capabilities.
This source package is intended for an experimental public configuration repository.
It is not a consumer Gemini app listing or a tested gallery release.

[Connector documentation](https://peoplescourt.ai/docs/api/connectors) explains the supported workflow.
Support: contact@peoplescourt.ai.

## Connection and personal authority

This package targets Gemini CLI `0.64.0` and uses `url` with `type: "http"` for Streamable HTTP.
Gemini CLI supports OAuth discovery from the service’s authentication response and metadata.
This draft relies on that discovery rather than embedding client credentials, scopes, or authorization endpoints.
Availability of a compatible registration method, loopback callback, scope grant, and issuer response remains to be verified in the actual Gemini CLI host.
OAuth sign-in grants account access within approved scopes; it does not accept arbitration or authorize a fee.
Each person must personally accept their exact filing or Answer packet on People’s Court.
Fees are completed by the person through People’s Court; this extension’s context does not authorize agent payment operations.
Review the service’s privacy notice and the assistant provider’s terms before sharing case information.
Context instructions supplement service authorization; they are not a technical access-control mechanism.

## Local setup and remaining verification

The local extension link and terminal `gemini mcp list` were run in a dedicated empty temporary workspace with an isolated `GEMINI_CLI_HOME`.
The person testing their own account must still complete authenticated verification.
Review and complete the browser authorization yourself when testing.
For the local package layout below, run from the parent of the `peoples-court` directory.
For a repository checkout, name the checkout directory `peoples-court` to match the manifest name, or substitute its absolute path:

```sh
gemini extensions link ./peoples-court
```

Restart Gemini CLI and inspect `/extensions list` and `/mcp list`.
If authentication is required, the person can initiate `/mcp auth peoples-court` and review the browser authorization screen.
Verify read-only tool discovery and an authorized read on the person’s own account before considering any write workflow.
Any filing, declaration, consent, payment, or case action retains its separate personal-authority requirements.
Do not use consent-skipping or automatic-approval flags.

## Release prerequisites

Public source publication is authorized for this experimental configuration only.
Do not create a GitHub Release or add the `gemini-cli-extension` gallery topic until authenticated host tests pass.
Before a gallery release or GitHub Release, verify the actual host/OAuth flow, token refresh and revocation, scope behavior, tool confirmations, refusal handling, and the personal-acceptance boundary using authorized test data.
No open-source license has been selected or granted for this package.
Public availability does not itself grant a software license.
Repository ownership and publication are separate release actions.
For a later gallery release, keep `gemini-extension.json` at the public repository’s absolute root and add the `gemini-cli-extension` topic only after authenticated host verification.
The official gallery discovers tagged repositories and validates them; discovery is not a promise of acceptance or timing.
After authenticated host verification and release approval, a GitHub Release may distribute the supplied ZIP as a generic platform-independent asset, with the manifest at archive root and a release tag matching version `0.1.0`.
The experimental public source repository is [shunhe-wang/peoples-court-gemini-cli](https://github.com/shunhe-wang/peoples-court-gemini-cli).
No GitHub Release, gallery topic, signup, service terms acceptance, gallery submission or OAuth grant was created during package preparation and the isolated host probe.

## Source and verification record

Checked October 16, 2026 against current official sources:

- [Extension reference](https://geminicli.com/docs/extensions/reference/): root manifest, lowercase dash-separated name, version, server map, context file; extensions do not support the server `trust` option.
- [MCP server configuration](https://geminicli.com/docs/tools/mcp-server/): OAuth discovery and dynamic registration are supported when the server provides them.
- [Release guide](https://geminicli.com/docs/extensions/releasing/): public GitHub repository, topic and root manifest for gallery discovery; ZIP release assets require a root manifest.
- [Pinned ExtensionConfig interface](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/cli/src/config/extension.ts#L24) and [MCPServerConfig](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/core/src/config/config.ts#L484): fields used here exist in upstream types.

The pinned `v0.64.0` source supports `url` with `type: "http"` and marks `httpUrl` deprecated.
The package uses the preferred fields.
Gemini CLI `0.64.0` was installed from the official `@google/gemini-cli` npm package in an isolated temporary directory with install scripts disabled.
`gemini --version` returned `0.64.0`.
`gemini extensions validate <package-directory>` completed successfully with exit status 0 using an isolated `GEMINI_CLI_HOME`.
JSON syntax, package directory/name, context presence, and ZIP contents were also checked.
The follow-up isolated host probe approved workspace trust only for its dedicated empty workspace and the reviewed configuration extension.
`gemini extensions link <package-directory>` reported that `peoples-court` was linked successfully and enabled.
`gemini mcp list` recognized `https://mcp.peoplescourt.ai/mcp` as an HTTP server from the extension and reported **Disconnected**, with command exit status 0.
The normal command output did not expose the connection failure reason.
The command ran without model authentication; no model sign-in or model prompt was attempted.
The [pinned status-command implementation](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/cli/src/commands/mcp/list.ts) attempts MCP connection and optional ping, not `tools/list`.
These results establish local package validation and host configuration recognition, not a successful service connection, authenticated compatibility or gallery acceptance.
OAuth authorization, authenticated tool discovery, account reads, token refresh and revocation, personal acceptance and case workflows remain untested.
