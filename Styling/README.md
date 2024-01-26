# Web Styling

# 1. Vendor Prefix
- Prefixes required for adding CSS features which are not standard (eg. experimental, min-version compatibility)
- Vendor prefixes are no longer required if browser version is updated and the CSS feature is offically supported.
- Adding vendor prefix increase the size of the code and requires effort, therefore the task is usually done by dedicated libraries. (eg. `prefixfree`, `autoprefixer`)
- List of browser and corresponding vendor prefixes:
<table>
    <thead>
        <tr>
            <th>Browser</th>
            <th>Vendor Prefix</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>IE or Edge</td>
            <td>-ms</td>
        </tr>
        <tr>
            <td>Firefox</td>
            <td>-moz-</td>
        </tr>
        <tr>
            <td>Opera</td>
            <td>-o-</td>
        </tr>
        <tr>
            <td>Chrome, Safari (including mobile)</td>
            <td>-webkit-</td>
        </tr>
    </tbody>
</table>