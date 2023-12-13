# Code Flow documentation

## Search

1. When you click the search button in the `/search.html` page of the extension, it triggers [addEventListener('submit', ...](extension/src/search.js#L145)
2. this further calls `doSearch` with an argument [`allsources.search(getQuery().value),`](](extension/src/search.js#L152)).
3. the `allsources.search` basically is a pointer to [`await MultiSource.get()).search(url)`](extension/src/sources.js#L426)
4. which again is a pointer tp [`async search(url: Url): Promise<Visits | Error> {]`](extension/src/sources.js#L359)
5. the above method calls `await _merge(url, ...this.sources.map(s => s.search(url)))`, where `s.search` is a pointer to [`search: async function(url: Url): Promise<Visits | Error> {`](extension/src/api.js#L104)
6. which calls the [`queryBackendCommon`](extension/src/api.js#L24) method.
7. which goes to line [``const endpoint = `${opts.host}/${endp}` ``](extension/src/api.js#L52) - which basically calls the api endpoint `http://localhost:13131/search`
8. the end point is handled in the server here - [`@app.post('/search', response_model=VisitsResponse)`](src/promnesia/server.py#L268)
9. this calls the [search_common](src/promnesia/server.py#L156) method
10. which extracts the relevant rows from the indexed db here [`query = table.select().where(where(table=table, url=url))`](src/promnesia/server.py#L169)
11. Once the server responds with data it goes to [`.then(rawToVisits)`](extension/src/api.js#L106), which is after point number 5 above.
12. which calls the [rawToVisits](extension/src/api.js#L156) method to convert the json from server to a class