<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { causeOf, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  let { ticket }: { ticket: number } = $props();

  const merged = $derived(uniqueById(reports as Report[]).filter((r) => r.mergedInto === ticket));
  // A one-day move is a different symptom from a flipped date, whatever the tenant's locale says.
  const oneDay = $derived(merged.filter((r) => notes.oneDayMove.includes(r.id)));
  const rest = $derived(merged.filter((r) => !notes.oneDayMove.includes(r.id)));
  const parser = $derived(rest.filter((r) => causeOf(r, tenantOf(r)) === 'parser'));
  const hint = $derived(rest.filter((r) => causeOf(r, tenantOf(r)) === 'hint'));
  const unplaced = $derived(rest.length - parser.length - hint.length);
  const span = (rs: Report[]) => {
    const d = rs.map((r) => r.receivedAt.slice(0, 10)).sort();
    return `${d[0]} → ${d[d.length - 1]}`;
  };
  const tenants = (rs: Report[]) => new Set(rs.map((r) => r.tenant)).size;
</script>

<p><strong>{merged.length}</strong> reports merged into #{ticket} · <strong>3</strong> problems{unplaced ? ` · ${unplaced} unplaced` : ''}</p>

<table>
  <thead><tr><th>problem</th><th>reports</th><th>tenants</th><th>seen</th></tr></thead>
  <tbody>
    <tr><td>parser reads dates month first</td><td>{parser.length}</td><td>{tenants(parser)}</td><td>{span(parser)}</td></tr>
    <tr><td>booking-v3 form hint says MM/DD/YYYY</td><td>{hint.length}</td><td>{tenants(hint)}</td><td>{span(hint)}</td></tr>
    <tr><td>date moves by one day</td><td>{oneDay.length}</td><td>{tenants(oneDay)}</td><td>{span(oneDay)}</td></tr>
  </tbody>
</table>
