---
name: "monthly-pharma-sales-consolidation"
description: "Use when Qasim sends monthly distributor sale PDFs (Pakistan pharma region) to merge into one master Excel with city-wise units, T.P, value, growth."
---

# Monthly distributor PDFs -> one master Excel

User: Qasim, works for High-Q (Islamabad region, manager Junaid Akhtar). Reply in Roman Urdu. Keep replies short.

## Input
- Text (not scanned) PDFs from distributor software (4M Technologies, Quirky Tech, Datalink, clicknet). Several PDFs can belong to one city (Pharma / Nutrition / Vida): Attock, Wah, Skardu, Nimra. All go in that city's one column.
- Check the date range in each PDF header first. Tell the user if it is not a full month (e.g. 1-18 Nov) or differs from other files, and that growth needs the same period of last year.

## User's rules (do not change)
1. Take net sale units from every PDF row (sales minus returns; keep negative numbers).
2. Same product in many cities = ONE row. A product sold in only some cities is still listed.
3. T.P comes from the PDF rate. Same rate = same row. A different rate = separate row of that product.
4. Names differ across PDFs (Tab/Cap/Inj/Syp, pack size, spelling). Treat as same product by name + rate. Ignore form words and pack text.
5. Value = units x T.P (Excel formula, not typed numbers).
6. Shifa and Khan House are entered manually by the user. Bagh, Nimra etc. come as PDFs when the user has them. Leave missing cities as empty green input columns.
7. Growth = this period vs same period last year (city-wise) plus a total. The 2025 figures come from the user: leave blank input row, growth by formula.

## Excel layout (copy the user's March sheet style)
Sheet Sales: row1 manager/title, row2 region, row3-4 headers, col A S.NO, B product, C T.P, then for each city two columns (Units, Value), then TOTAL Units/Value, TOTAL row, then SALES AMOUNT 2026, SALES AMOUNT 2025 (input), GROWTH rows. City order: NIMRA, SHIFA, KHAN HOUSE, SKARDU, ATTOCK, WAH, CHAKWAL, MIRPUR, KOTLI, RAWALAKOT, BAGH, MUZAFFARABAD, GILGIT, SARGODHA, MIANWALI, JHELUM (Jhelum is new, Ideal Traders). Add any new city as a new column and tell the user.
Sheet Summary: city-wise 2026, 2025, growth %, PDF total (check), difference + a bar chart.
Sheet Review: every row with the PDF names it came from. Yellow = new product or rate not in the March list; blue = name matched but rate differs from March.
Sort rows alphabetically. Recalculate with LibreOffice (soffice --headless --convert-to xlsx) so cached values show on phone, then send that file.

## Parsing (what worked)
- pdftotext -layout file.pdf -. Product line regex: optional code, name, 2+ spaces, optional pack, rate ([\d,]+\.\d{1,2}), then numbers. Fallbacks needed: rate only ONE space after a long name; pack placed after the rate (e.g. 1X10, 20`S); pack with a space (6 AMP, 30 TAB) -> drop 2 tokens; item name wrapped onto the previous line when the name looks like a pack only (Skardu). Skip lines whose name contains total/purchase/grand.
- Sale columns are position based (0-indexed numeric tokens after rate/pack), left-aligned: 4M Technologies Wah/Attock/Gilgit = (units 12, value 14); Kotli, Mirpur, Chakwal, Sargodha, Rawalakot, Nimra, Jhelum = (8, 10); Muzaffarabad (Four Star) = (13, 15); Skardu (AL Shifa, clicknet) = (5, 6) net sale; Mianwali (Datalink) has blank columns, so use the first (u, v) pair with u x rate approximately v (tolerance max(1.5, 0.06% of v)).
- Never pick the first matching pair blindly on dash formats: opening stock x rate can equal closing value. Use the fixed index. For a new/unknown format, infer indexes by counting pairs where u x rate approximately v across rows and take the earliest column pair with >= 60% of the max count (the closing pair always matches, the sale pair comes before it), then verify against the PDF total.
- If u x rate differs from v by more than the tolerance for a row, flag it.

## Name/rate matching
Normalise: lowercase, remove words (tab, cap, inj, syp, susp, cream, infusion, sachet, softgel, ampoule...), pack like 1x10 and 10's, convert g to mg, keep IV/IM tokens (Dayline-IV and -IM have the same rate but are different products). Group rows into rate buckets (within 0.05). Inside a bucket merge two names if: IV/IM do not conflict, numeric strengths are not conflicting (one set must be a subset of the other), and similarity >= 0.72 (difflib ratio, or 0.8 if the first word is the same/prefix). Display name: the user's March sheet product name with matching rate; else name-only match >= 0.85 (blue); else most common PDF name (yellow). Only include rows with non-zero net units and rate > 1.5.

## Always reconcile
Sum of parsed sale values per PDF must equal the PDF's own total sale value (found in the Total / Report Total line, within about Rs 10). Then also compare the final Excel city value (units x T.P) with the PDF total (difference is only rate merging, under about Rs 100). Report results in the reply. If a PDF does not reconcile, find the cause before sending. Known: Jhelum's PDF lists rows that sum to Rs 243,198 less than its own printed total (rows hidden in report) - tell the user and use printed rows.

## Past findings
- Feb 2026 PDFs checked: all 17 reconcile. March 2026 manual sheet (the layout reference) had 204 product rows.
- User's manual Feb sheet gave Mianwali 29,750,209 vs PDF total 29,735,642 (Rs 14,567 higher). Manual sheet needed to find the product-wise difference.
- Nov 2026 (1-18 Nov) built with 9 cities; Attock, Mirpur, Rawalakot, Gilgit, Bagh, Shifa, Khan House had no PDFs.

## Next steps
Growth chart (this month vs same month last year, city-wise, plus yearly city total) and a PowerPoint from the same data, then email replies. Gmail/Outlook access and scheduled monthly runs need connectors or a paid plan; tell the user, do not pretend.

## Delivery
Send the Excel with SendUserFile, then a short Roman Urdu summary: cities included/missing, reconciliation, rows needing review, new cities.