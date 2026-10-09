# UK Job Ad Checker

An Intelligent Contract on GenLayer that checks UK job adverts for scams and unfair terms.

You paste in a job advert. The contract asks an AI to review it the way an experienced UK recruiter would. It decides whether the ad looks legit, suspicious or like a scam, checks the pay against the minimum wage, and for security jobs checks whether an SIA licence is mentioned. The result is saved on-chain so it can't be quietly changed later.

## Why I built it

When I was looking for security work in the UK, I came across plenty of adverts that didn't add up. Some asked for money upfront for training or a DBS check, some paid below minimum wage, and some security roles never mentioned the SIA licence the job legally needs. People new to the UK job market are especially easy targets. This contract gives job seekers a quick, consistent second opinion before they apply or hand over any personal details.

## What it checks

For every advert, the contract returns:

- **Verdict:** `legit`, `suspicious` or `likely_scam`
- **Trust score:** 0 to 100
- **Pay check:** `above_minimum`, `below_minimum` or `not_stated`, compared with the UK National Living Wage (£12.71 an hour for workers aged 21 and over from April 2026)
- **SIA licence:** `required_and_mentioned`, `required_but_missing` or `not_applicable`
- **Red flags:** up to 5 specific warning signs found in the ad
- **Advice:** one sentence for the job seeker

## How consensus works

Every validator on GenLayer checks the advert with its own AI model. The leader proposes a result, and the other validators compare it with their own using GenLayer's comparative equivalence principle. They must agree on the **verdict**. The score, red flags and wording are allowed to differ slightly, because two AIs will never word things exactly the same way.

After consensus, plain Python cleans up the result before it's stored. The advert text is treated strictly as data, so an advert can't sneak instructions to the AI.

## Methods

| Method | Type | What it does |
|---|---|---|
| `check_job_ad(job_ad)` | write | Checks an advert (30 to 5000 characters) and saves the result |
| `get_check_count()` | view | Number of adverts checked so far |
| `get_check(check_id)` | view | One check by id, starting from 0 |
| `get_latest_check()` | view | The most recent check |

## Try it in GenLayer Studio

1. Open [GenLayer Studio](https://studio-next.genlayer.com). The contract targets GenVM v0.2.16.
2. Upload `job_ad_checker.py` and click Deploy. There are no constructor parameters.
3. Under Write Methods, call `check_job_ad` with a test advert, for example:

   > URGENT! Security guards needed in London, start tomorrow. £25 per hour, no experience needed. Pay £85 for your training pack and DBS check before your first shift. WhatsApp Dave on 07700 900123 to secure your place today.

4. Wait for the transaction to finalise, then call `get_latest_check` under Read Methods.

Try a genuine-looking advert too, with a named company, realistic pay and an SIA licence requirement, and compare the results.

## Limitations

- This is a second opinion, not legal advice. Always research the employer yourself.
- The minimum wage figure is set in the contract and needs updating each April.
- Adverts are stored publicly on-chain, so don't include anyone's personal contact details from real adverts.

## Built with

- [GenLayer](https://docs.genlayer.com) Intelligent Contracts (Python)
- Developed with help from Claude
