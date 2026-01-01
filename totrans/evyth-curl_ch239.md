# Stop slow transfers

By default, a transfer can stall or transfer data extremely slow for any period without that being an error.

Stop a transfer if below **N** bytes/sec during **M** seconds. Set **N** with `CURLOPT_LOW_SPEED_LIMIT` and set **M** with `CURLOPT_LOW_SPEED_TIME`.

Using these option in real code can look like this:

[PRE0]