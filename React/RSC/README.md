# RSC (React Server Component)

### 1. RSC vs RCC
<table>
	<thead>
	<tr>
		<th> </th>
		<th>RSC<br>(React Server Component)</th>
		<th>RCC<br>(React Client Component)</th>
	</tr>
	</thead>
	<tbody>
	<tr>
		<th>Rendered Environment</th>
		<td>Server</td>
		<td>Client</td>
	</tr>
	<tr>
		<th>Deliver Format<br>(to client)</th>
		<td>Serialized output in chunks (RSC Payload)</td>
		<td>HTML + JS bundle</td>
	</tr>
    <tr>
        <th>Can return promises</th>
        <td>O</td>
        <td>X</td>
    </tr>
	<tr>
		<th>Rendering process</th>
		<td>
			(1) Rendered as `template` with id<br><br>
			(2) Provide suspense fallback why asynchronous rendering is in progress<br><br>
			(3) Processed output is serialized and sent to the client (client replaces with the received version)
		</td>
		<td>
			(1) HTML + JS bundle is sent <br><br>
	    	(2) Client hydrates the component to make interactive
		</td>
	</tr>
	<tr>
	<th>Advantages</th>
	<td>
	- Enables direct access to server resources (eg. DB) which reducecs amount of codes and network overhead, ensures security.<br><br>
	- Can optimize module dependencies because the required modules are called from the server (whereas client components need to fetch the dependencies through network)<br><br>
	- Reduces network payload (direct resource access, package dependencies, no need for JS bundles, ..)
	- Caching: result of the requests could be cached and shared across users.
	</td>
	<td>
	- Can be updated and use React life cycle (eg. using Hooks, event listeners are all disabled from Server Components) -> Enables interactivity<br><br>
	- Can utilize Browser APIs
	</td>
	</tr>
	</tbody>
</table>


### 2. Overall process of RSC + RCC rendering
    - Server receives a request
    - Server serializes the root component in a form of JSON tree
    - Based on the JSON, server creates HTML (If RSC in the tree returns `Promise`, it is filled with template which will be replaced with actual render output of RSC)
    - Server sends client HTML + JS bundle required for Client Component rendering
    - Client starts rendering client components by hydration
    - Server sends RSC payload when asynchronous RSC rendering is complete (each independently)
    - Client parses the payload and updates DOM tree

### 3. RSC Payload
- Representation of rendered React Server Component tree.
- Is sent from server to client to apply render output of RSC.
- RSC Payload contains:
    - Render result of RSC
    - Placeholders for client components included in the tree, and references to their JS files (basically information needed to render client components included in the server component)
    - Props that RSC needs to pass to client component


### 4. React Query vs RSC
- Most of the React Query use cases could be replaced with RSC
    - Queries: with Initial data or Hydrate
    - Mutation: with server action
- Some features of React Query are not replacable with RSC
    - Infinite scrolling items
    - Interval refetch (showing fresh data even without user interactions)
    - Offline queries that do not require network
    - Environments outside Web (eg. React Native)