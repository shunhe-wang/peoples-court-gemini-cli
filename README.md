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
This draft discovers endpoints and pins the requested scope to `cases:read`, without embedding client credentials.
It sets `oauth.enabled` to `false` to disable the CLI’s automatic initial-authentication branch; the intended initial grant uses `/mcp auth peoples-court` explicitly.
In pinned `0.64.0`, the explicit command passes configured scopes to the OAuth provider and its discovery merge preserves them.
Automatic authentication takes scopes from authorization-server metadata instead, so enabling it is not an equivalent least-privilege configuration.
This initial-auth behavior is source-inspected and package-validated; no live authorization grant has verified it.
An initial manual read-only test may proceed in a fresh isolated profile after the person reviews the exact account, client recipient, `cases:read` scope and grant duration.
Do not approve a broader scope or an unexpected registration or authentication challenge.
Availability of a compatible registration method, loopback callback, scope grant, and issuer response remains to be verified in the actual Gemini CLI host.
OAuth sign-in grants account access within approved scopes; it does not accept arbitration or authorize a fee.
Each person must personally accept their exact filing or Answer packet on People’s Court.
Fees are completed by the person through People’s Court; this extension’s context does not authorize agent payment operations.
Review the service’s privacy notice and the assistant provider’s terms before sharing case information.
Context instructions supplement service authorization; they are not a technical access-control mechanism.

## Local setup and remaining verification

The local extension link and terminal `gemini mcp list` were run in a dedicated empty temporary workspace with an isolated `GEMINI_CLI_HOME`.
The person testing their own account must still complete authenticated verification.
For an explicitly authorized initial read-only test, personally review the browser consent details before granting access; keep subsequent reauthorization testing separate.
For the local package layout below, run from the parent of the `peoples-court` directory.
For a repository checkout, name the checkout directory `peoples-court` to match the manifest name, or substitute its absolute path:

```sh
gemini extensions link ./peoples-court
```

Restart Gemini CLI and inspect `/extensions list` and `/mcp list`.
After the exact test account, client recipient, `cases:read` scope and grant duration have been reviewed, the person may initiate `/mcp auth peoples-court` for the bounded initial test.
Stop if the requested access or registration differs from that reviewed test.
Verify read-only tool discovery and an authorized read on the person’s own account before considering any write workflow.
The intended initial manual authorization requests `cases:read` only.
This does not establish a complete scope boundary for stored-token reuse or reauthorization.
Filing or other writes require a separately reviewed configuration with only the exact required scopes and a separate, personally approved grant, preferably in a distinct isolated profile.
An existing read grant does not expand when a configuration file changes.
Automatic scope step-up and write workflows have not been established for this package.
Keep automatic authentication disabled; do not select all advertised API scopes.
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
That earlier status probe ran without model authentication; it attempted no model sign-in or model prompt.
The [pinned status-command implementation](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/cli/src/commands/mcp/list.ts) attempts MCP connection and optional ping, not `tools/list`.
These results establish local package validation and host configuration recognition, not a successful service connection, authenticated compatibility or gallery acceptance.
People’s Court OAuth authorization, authenticated tool discovery, account reads, token refresh and revocation, personal acceptance and case workflows remain untested.

## Least-privilege source record

The [manual auth command](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/cli/src/ui/commands/mcpCommand.ts#L118) passes the configured OAuth object to the provider.
The provider preserves explicit scopes in both [challenge discovery](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/core/src/mcp/oauth-provider.ts#L340) and [standard discovery](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/core/src/mcp/oauth-provider.ts#L376).
The [connection path](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/core/src/tools/mcp-client.ts#L1989) refuses automatic authentication when `oauth.enabled` is false.
The [automatic-auth path](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/core/src/tools/mcp-client.ts#L948) otherwise uses discovered scopes instead of the configured scope list.
The authorization server advertises scopes beyond the MCP resource, including fee authority; these are not requested by this manual read-only configuration.
The previous link/status probe preceded this scope restriction; it reported Disconnected and created no grant.
The restricted candidate has passed CLI package validation only; its live grant, tool authorization and reauthentication behavior remain untested.

## Stored-token limitation

`oauth.enabled: false` does not block reuse of a stored token.
The pinned [transport code](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/core/src/tools/mcp-client.ts#L2302) selects the stored-token provider when credentials exist.
The [stored-token provider](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/core/src/mcp/stored-token-provider.ts#L53) uses the saved token and the [refresh helper](https://github.com/google-gemini/gemini-cli/blob/v0.64.0/packages/core/src/mcp/oauth-provider.ts#L695) uses its saved client and token endpoint without repeating metadata discovery.
An ordinary refresh omits a new scope request when automatic authentication is disabled.
However, a subsequent HTTP authentication challenge can enter the underlying MCP SDK’s separate reauthorization path, whose provider does not preserve the configured scope cap or persisted client identity.
That SDK path can attempt new client registration and construct an authorization URL using scopes from resource metadata.
The actual CLI bundle’s stored-token provider implements `redirectToAuthorization` and `saveTokens` as no-ops and supplies no callback URL, so this path cannot open a browser or complete a broader authorization grant.
The observed offline result is a possible registration attempt followed by unusable reauthorization, not automatic expansion of approved access.
A registration’s supported-scope ceiling is distinct from the scopes a person approves for a grant; a broader ceiling does not itself create a broader grant.
Initial manual `cases:read` testing remains conditional on the personal consent review above.
Reauthorization failure, refresh and revocation remain live-test limitations; stop at an unexpected challenge or registration and do not infer a complete end-to-end scope boundary.
An offline, network-disabled probe of the pinned authorization-URL builder serialized exactly `cases:read` with S256 PKCE; it did not register a client, create a grant, or exercise that reauthorization path.

## Interactive model-access test

On October 16, 2026, an actual official Gemini CLI `0.64.0` session was started in a fresh empty isolated workspace/profile, with no MCP server configured and telemetry disabled.
The person selected their Google account and completed browser OAuth.
The CLI then rejected eligibility with: “This client is no longer supported for Gemini Code Assist for individuals” and directed migration to Antigravity.
The session showed zero tool calls, zero agent activity and zero model API calls; no model prompt or People’s Court grant was submitted.
This is a host model-access eligibility block before connector testing, not a People’s Court connection failure.

Google’s [consumer deprecation notice](https://developers.google.com/gemini-code-assist/docs/deprecations/code-assist-individuals?hl=en), updated October 9, 2026, says consumer-tier CLI service via Google login ended June 18, 2026.
The generic [CLI authentication guide](https://geminicli.com/docs/get-started/authentication/) still recommends individual Google login, so those official pages conflict on that route; the observed rejection matches the specific deprecation notice.
The documented Gemini API-key route remains a pending alternative, not a tested success or a promise of free usage.
AI Studio’s initial terms screen remains unaccepted for personal review; no new project, API key or billing enrollment was created in this test.
No gallery topic or GitHub Release has been enabled.
