<script>
  import Footer from '$lib/Footer.svelte';
</script>

# {$page.params.country}

```sessions_by_month_by_country
SELECT 
date_trunc('month',START_TSTAMP) as month_mmmyy, 
geo_country,
count(1) AS number_of_sessions
FROM atomic_derived.snowplow_mobile_sessions
-- filter data range, if needed, example: WHERE START_TSTAMP BETWEEN DATEADD(day, -7, GETDATE()) AND  DATEADD(day, -1, GETDATE())
group BY 1,2
order BY 2,1
```

<BarChart 
    data={sessions_by_month_by_country.filter(d => d.geo_country === $page.params.country)} 
    title="Sessions by Month, {$page.params.country}"
/>


```sessions_by_city
select
geo_country,
geo_city,
count(1) AS number_of_sessions,
rank() over (partition by geo_country order by count(1) desc) as city_rank
from atomic_derived.snowplow_mobile_sessions
group by 1,2
order by 1,3 desc
```

The top {$page.params.country} cities by sessions are:

{#each sessions_by_city.filter(d => d.geo_country === $page.params.country) as city_row}
{#if city_row.city_rank <= 3}
- {city_row.geo_city} ({city_row.number_of_sessions})

{/if}
{/each}
<Footer prev="/3._visualisation" prevLabel="Back to Visualisation"/>

<style>
  ul {
    margin: 0;
  }
</style>
