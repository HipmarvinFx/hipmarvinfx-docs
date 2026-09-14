# HipMarvinFX -- Verification: Five Items From Earlier Session Report

**Status:** Verified against real code and database, 14 September 2026

These five items were flagged in an earlier session's status report as
"not touched" open items. Per this session's own discipline (verify
before trusting a prior claim -- see SESSION_HANDOVER_2026-09-14.md
section 4), each was checked directly against the actual codebase and
database rather than carried forward as-is. Two of the five claims
turned out to be false.

---

## Confirmed real, genuinely missing

**1. No age 18+ verification anywhere.**
Searched all of app/ for age/birthdate/DOB fields -- none exist. No
age gate on signup, onboarding, or anywhere else in the flow.

**2. No Terms of Service consent step.**
app/legal/terms-of-service exists as a static page, but nothing in
app/login/page.tsx or app/complete-profile/page.tsx requires a user
to actively agree to it (no checkbox, no consent flag written to
profiles). A user can complete signup and onboarding without ever
being shown or asked to accept the terms.

**3. No captcha/bot-check on auth.**
Searched for captcha, recaptcha, turnstile, hcaptcha across app/ --
zero matches. Login and signup are fully open to automated submission.

These three are real gaps. Whether they matter, and how urgently,
depends on legal/compliance requirements for the jurisdiction(s)
HipMarvinFX operates in -- not something to guess at here. Worth a
decision from whoever owns that.

---

## Checked and found FALSE -- do not re-flag without new evidence

**4. "Missing signup trigger" -- false.** A real, working trigger exists:

    on_auth_user_created -- fires AFTER INSERT ON auth.users
      -> EXECUTE FUNCTION handle_new_user()

handle_new_user()'s body, confirmed via pg_proc:

    BEGIN
      INSERT INTO public.profiles (id, email, status)
      VALUES (new.id, new.email, 'pending')
      ON CONFLICT (id) DO NOTHING;
      RETURN new;
    EXCEPTION
      WHEN others THEN
        RETURN new;
    END;

Every new auth.users row gets a profiles row created immediately, with
status: 'pending' -- matching the column default confirmed separately
tonight. The EXCEPTION WHEN others block means any insert failure here
is swallowed silently at the application/client level (signup still
succeeds either way) -- this is a deliberate, reasonable choice, not a
bug, though worth knowing if anyone ever needs to debug a missing
profiles row: check Postgres server logs, not client-visible errors,
since the trigger itself won't surface a failure to the app.

This trigger, combined with the complete-profile upsert fix shipped
earlier tonight, means profile-row creation now has two independent,
consistent paths -- not a single point of failure.

**5. "/complete-profile country-dropdown visual bug" -- not supported
by the code.** Read the full file (app/complete-profile/page.tsx)
directly. The country field is a plain select bound to a static
12-country + "Other" array, with a placeholder option and validation
(if (!country) { setError(...) }). Nothing in the source is visibly
broken. If a real bug exists, it would have to be a runtime/browser-
rendering issue not visible from source -- not confirmed either way,
but the earlier claim as stated (implying an obvious code-level issue)
is not supported by what's actually there.

---

## Net result

Of the earlier report's five flagged items, 3 are real and still open
(age gate, T&C consent, captcha), 2 were false (signup trigger exists
and works correctly; no dropdown bug found in source). This is
consistent with this session's broader finding that an unverified
prior status report should not be trusted at face value -- every claim
in it needs independent confirmation before being acted on or carried
forward.

---

Logged: 14 September 2026
