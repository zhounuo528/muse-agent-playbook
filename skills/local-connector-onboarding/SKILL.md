# 连接器接入验收法【随仓附带】

# Connector Onboarding

## Verify every capability claim with observed evidence

- Search with `muse.skill_search` for the connector skill first and follow its tooling and auth sections as a starting point and never as a verdict.

- Treat another assistant description of the connector as a lead and never as evidence and confirm the auth scheme and endpoint behavior yourself.

- Confirm the endpoint is alive and the auth exactly as documented before you do anything else.

- Debug a 401 or 403 answer from the observed error itself and fix the host, header name, or key placement that the evidence shows.

- Run the connector tool listing or its equivalent before you describe any capability and record the observed inventory.

- Assert a tool exists only after it appears in that listing.

- Scope to least privilege and start with reads only and add write tools only after the user explicitly asks for them.

- Store secrets through the secure flow only and never print a key or token in chat, logs, or files.

- Perform one genuine read against the user own data before you declare the connector working.

## Test a write claim before you trust it

- Treat a documented dry run flag, preview mode, or idempotency key as an unverified claim and never as proof that a call cannot write.

- When another skill or connector doc prescribes a dry run as a safe precheck, this section takes precedence over that instruction until the dry run is proven with a small authorized test and a readback.

- Prove a dry run or idempotency claim with one small case that the user explicitly authorized and then read the result back before you use that claim for any larger job.

- Treat every write call as a real write until that small proof succeeds.

- Do not use an unproven dry run as a precheck for a batch, because a batch of supposed prechecks can become a batch of real writes.

- Read the affected day or record back after any uncertain write or dropped connection before you retry, because the first request may have completed.

- Retry only after the readback shows the record is missing and never retry blindly after a disconnect.

- Keep each authorized write small and check the count after it, so a duplicate shows up at once instead of after a full import.

- Preserve identifiers and times when you update a record and never delete an old record only because a read did not return it.

## Reset a stuck connector credential

- Treat a connector card that shows only an added state with no input box as a completed session being reused, not as a fresh setup.

- Check the credential the user pastes before you reissue a card and reject a key name or key identifier when the service expects a secret value.

- Ask the user to disconnect or delete that connector in the app settings first when a reissued card offers no input box or when a corrected key still returns the same authentication error.

- Issue a fresh connector card only after that disconnect and confirm the new card shows an input box before the user pastes anything.

- Run the same smoke read that failed before and read the result back before you declare the new credential working.

- Record the disconnect and the fresh card in the report so a later retry does not reuse the old session again.

## Handle OAuth callback links exactly

- Forward an OAuth authorization or callback link exactly as the system issued it and never retype, shorten or reassemble it by hand, because one changed character breaks the handshake without saying which character is wrong.

- Treat a bare callback address opened without the parameters from a live flow as expected to error and never present that error as evidence that the provider or the account is broken.

- Test the provider side on its own before you retry the platform step and check its registration, authorization and token endpoints where it exposes them, so the report can name which side actually failed.

- Stop after the same platform step fails across a second device, the original link and a parameter variant and save the full diagnostics, then offer a platform bug report once instead of asking the user to repeat the same handshake.

## Report what remains unverified

- List what was connected and the auth method used without the secret itself.

- List the observed tool inventory with its count and the names relevant to the task.

- List the smoke test evidence with the real read that succeeded.

- List any claimed but unverified capability with what would verify it.

- Correct an overstated claim in the same turn that evidence disproves it.

- Verify before you deny and run a non destructive test first and deny only when the safe test itself fails.

- Report a capability as unverified when no non destructive test exists and say what would verify it.

- Do not attempt a state changing action as a test unless the user explicitly asked you to perform that action and then perform only what was requested and read back the result.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Alternatives for better results

- Ask the user for the smallest authorized test record when no safe test exists.

- Use the connector own read path to verify state after a write instead of trusting command output alone.

- Split a large import into single day batches with a readback after each one, so a failure stays contained.
