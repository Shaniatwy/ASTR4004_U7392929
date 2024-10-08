# ASTR4004_U7392929 ASSIGNMENT3
## Task 2 - Gaia DR3 and 2MASS Crossmatch

This task involves querying the Gaia DR3 dataset and crossmatching it with the 2MASS catalog
to identify bright stars in the vicinity of the open cluster M67. The focus is on applying quality
cuts based on 2MASS photometry and Gaia parallaxes to narrow down the selection to stars with
reliable data.

## ADQL Query

The following ADQL query was used to search for bright stars around M67:

```sql
SELECT gaia.source_id, gaia.ra AS g_ra, gaia.dec AS g_dec,
       gaia.phot_g_mean_mag, gaia.bp_rp, gaia.parallax, gaia.pmra, gaia.pmdec,
       tmass.j_m, tmass.h_m, tmass.ks_m, tmass.ph_qual
FROM gaiadr3.gaia_source AS gaia
JOIN gaiadr3.tmass_psc_xsc_best_neighbour AS xmatch
  ON gaia.source_id = xmatch.source_id
JOIN gaiadr3.tmass_psc_xsc_join AS tmass_join
  ON xmatch.clean_tmass_psc_xsc_oid = tmass_join.clean_tmass_psc_xsc_oid
JOIN gaiadr1.tmass_original_valid AS tmass
  ON tmass_join.original_psc_source_id = tmass.designation
WHERE 1=CONTAINS(
    POINT('ICRS', gaia.ra, gaia.dec),
    CIRCLE('ICRS', 132.825, 11.8, 1))
AND gaia.phot_g_mean_mag < 14
ORDER BY gaia.source_id ASC;
```

## Results

- **Number of stars returned from the initial query:** 1018
- **Number of stars with bad 2MASS photometry (ph\_qual not 'AAA'):** 21
- **Number of stars with non-positive parallaxes:** 2
- **Number of stars remaining after applying quality cuts:** 988

## Conclusion
After applying quality cuts, 988 stars remain in the dataset. These quality cuts ensure that only stars with reliable 2MASS photometry and positive parallaxes are considered. This dataset can be used for further analysis, and the number of stars suggests a manageable number of fibres would be needed for spectroscopic follow-up.

## Plot

![Figure 1: Colour-magnitude diagram (CMD) of Gaia BP-RP vs. absolute G magnitude (left) and 2MASS J-Ks vs. apparent K magnitude diagram (right)](/Users/shaniatham/ASTR4004/astro_computing/ASTR4004_U7392929/figures/cmd_M67_subplot.png)

## Recommendation
Based on the number of stars remaining after applying quality cuts (988), I recommend moving forward with a proposal for spectroscopic fibre allocation. The reduced dataset ensures efficient use of fibres, focusing only on stars with good photometric and parallax data. By removing stars with bad 2MASS photometry and non-positive parallaxes, the dataset is refined, allowing the fibres to be allocated to scientifically valuable targets. The cuts reduce the number of stars from 1018 to 988, which makes fibre usage more efficient by ensuring that only stars with reliable data are observed. This approach balances the need to observe a substantial number of stars while maintaining the quality of the data, making the fibre allocation process more resource-efficient.
