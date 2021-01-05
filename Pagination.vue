<template>
	<div>
		<!-- Display Results -->
		<table class="w-full text-center my-8">
			<thead class="bg-gray-100">
				<th>Id</th>
				<th>Name</th>
				<th>Email</th>
				<th>Created at</th>
				<th>Updated at</th>
			</thead>
			<tbody>
				<tr v-for="item in items" :key="item.id">
					<td>{{ item.id }}</td>
					<td>{{ item.name }}</td>
					<td>{{ item.email }}</td>
					<td>{{ item.created_at }}</td>
					<td>{{ item.updated_at }}</td>
				</tr>
			</tbody>
		</table>
		<!-- End Display Results -->

		<!-- Pagination Nav -->
		<nav class="flex items-center justify-between">

			<!-- Previous/Next Buttons On Smaller Screens -->
			<div class="flex justify-between flex-1 sm:hidden">
				<span 
					v-if="current_page==1" 
					class="relative inline-flex items-center px-4 py-2 text-sm font-medium text-gray-500 bg-white border border-gray-300 cursor-default leading-5 rounded-md"
				>
                    &laquo; Previous
                </span>
                <a 
                	v-else 
                	@click="loadItems(prev_page_url)" 
                	class="relative inline-flex items-center px-4 py-2 text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 rounded-md hover:text-gray-500 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 cursor-pointer"
                >
                    &laquo; Previous
                </a>

                <a 
                	v-if="current_page < last_page" 
                	@click="loadItems(next_page_url)" 
                	class="relative inline-flex items-center px-4 py-2 ml-3 text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 rounded-md hover:text-gray-500 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 cursor-pointer"
                >
                    Next &raquo;
                </a>

                <span 
                	v-else 
                	class="relative inline-flex items-center px-4 py-2 ml-3 text-sm font-medium text-gray-500 bg-white border border-gray-300 cursor-default leading-5 rounded-md"
                >
                    Next &raquo;
                </span>
            </div>
            <!-- End Previous/Next Buttons On Smaller Screens -->

	        <div class="hidden sm:flex-1 sm:flex sm:items-center sm:justify-between">

            	<!-- Display Number Of Items -->
	            <div>
	                <p class="text-sm text-gray-700 leading-5">
	                    Showing
	                    <span class="font-medium">{{ from }}</span>
	                    to
	                    <span class="font-medium">{{ to }}</span>
	                    of
	                    <span class="font-medium">{{ total }}</span>
	                    results
	                </p>
	            </div>
	            <!-- End Display Number Of Items -->

		        <div>
	                <span class="relative z-0 inline-flex shadow-sm rounded-md">

	                	<!-- First Page Button -->
	                	<span v-if="current_page == 1">
	                        <span class="relative inline-flex items-center px-2 py-2 text-sm font-medium text-gray-400 bg-white border border-gray-300 cursor-default rounded-l-md leading-5">
	                            <svg class="w-5 h-5 -mr-1" fill="currentColor" viewBox="0 0 20 20">
	                                <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
	                            </svg>
	                            <svg class="w-5 h-5 -ml-2" fill="currentColor" viewBox="0 0 20 20">
	                                <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
	                            </svg>
	                        </span>
	                    </span>
	                        
	                    <a 
	                    	v-else 
	                    	@click="loadItems(first_page_url)" 
	                    	rel="prev" 
	                    	class="relative inline-flex items-center px-2 py-2 text-sm font-medium text-gray-500 bg-white border border-gray-300 rounded-l-md leading-5 hover:text-gray-400 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-500 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        <svg class="w-5 h-5 -mr-1" fill="currentColor" viewBox="0 0 20 20">
	                            <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
	                        </svg>
	                        <svg class="w-5 h-5 -ml-2" fill="currentColor" viewBox="0 0 20 20">
	                            <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
	                        </svg>
	                    </a>
	                    <!-- End First Page Button -->

	                    <!-- Previous Page Button -->
	                    <span v-if="current_page == 1">
	                        <span class="relative inline-flex items-center px-2 py-2 text-sm font-medium text-gray-400 bg-white border border-gray-300 cursor-default leading-5" aria-hidden="true">
	                            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
	                                <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
	                            </svg>
	                        </span>
	                    </span>
	                        
	                    <a 
	                    	v-else 
	                    	@click="loadItems(prev_page_url)" 
	                    	rel="prev" 
	                    	class="relative inline-flex items-center px-2 py-2 text-sm font-medium text-gray-500 bg-white border border-gray-300 leading-5 hover:text-gray-400 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-500 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
	                            <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
	                        </svg>
	                    </a>
	                    <!-- End Previous Page Button -->

	                    <!-- Page Buttons -->
	                    <a 
	                    	v-if="current_page > last_page-1 && current_page-4 > 0" 
	                    	@click="loadItems(custom_url+(current_page-4))" 
	                    	class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 hover:text-gray-500 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        {{ current_page-4 }}
	                    </a>

	                    <a 
	                    	v-if="current_page > last_page-2 && current_page-3 > 0" 
	                    	@click="loadItems(custom_url+(current_page-3))" 
	                    	class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 hover:text-gray-500 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        {{ current_page-3 }}
	                    </a>

	                    <a 
	                    	v-if="current_page-2 > 0" 
	                    	@click="loadItems(custom_url+(current_page-2))" 
	                    	class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 hover:text-gray-500 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        {{ current_page-2 }}
	                    </a>

	                    <a 
	                    	v-if="current_page-1 > 0" 
	                    	@click="loadItems(custom_url+(current_page-1))" 
	                    	class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 hover:text-gray-500 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        {{ current_page-1 }}
	                    </a>

	                    <span>
	                        <span class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium text-gray-500 bg-white border border-gray-300 cursor-default leading-5">{{ current_page }}</span>
	                    </span>

	                    <a 
	                    	v-if="current_page+1 <= last_page" 
	                    	@click="loadItems(custom_url+(current_page+1))" 
	                    	class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 hover:text-gray-500 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        {{ current_page+1 }}
	                    </a>

	                    <a 
	                    	v-if="current_page+2 <= last_page" 
	                    	@click="loadItems(custom_url+(current_page+2))" 
	                    	class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 hover:text-gray-500 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        {{ current_page+2 }}
	                    </a>

	                    <a 
	                    	v-if="current_page < 3 && current_page+3 <= last_page" 
	                    	@click="loadItems(custom_url+(current_page+3))" 
	                    	class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 hover:text-gray-500 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        {{ current_page+3 }}
	                    </a>

	                    <a 
	                    	v-if="current_page == 1 && current_page+4 <= last_page" 
	                    	@click="loadItems(custom_url+(current_page+4))" 
	                    	class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 hover:text-gray-500 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        {{ current_page+4 }}
	                    </a>
	                    <!-- End Page Buttons -->

	                    <!-- Next Page Button -->
	                    <a 
	                    	v-if="current_page < last_page" 
	                    	@click="loadItems(next_page_url)" 
	                    	rel="next" 
	                    	class="relative inline-flex items-center px-2 py-2 -ml-px text-sm font-medium text-gray-500 bg-white border border-gray-300 leading-5 hover:text-gray-400 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-500 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
	                            <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
	                        </svg>
	                    </a>
	                    <span v-else>
	                        <span class="relative inline-flex items-center px-2 py-2 -ml-px text-sm font-medium text-gray-400 bg-white border border-gray-300 cursor-default leading-5">
	                            <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
	                                <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
	                            </svg>
	                        </span>
	                    </span>
	                    <!-- End Next Page Button -->

	                    <!-- Last Page Button -->
	                    <a 
	                    	v-if="current_page < last_page" 
	                    	@click="loadItems(last_page_url)" 
	                    	rel="next" 
	                    	class="relative inline-flex items-center px-2 py-2 -ml-px text-sm font-medium text-gray-500 bg-white border border-gray-300 rounded-r-md leading-5 hover:text-gray-400 focus:z-10 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-500 transition ease-in-out duration-150 cursor-pointer"
	                    >
	                        <svg class="w-5 h-5 -mr-1" fill="currentColor" viewBox="0 0 20 20">
	                            <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
	                        </svg>
	                        <svg class="w-5 h-5 -ml-2" fill="currentColor" viewBox="0 0 20 20">
	                            <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
	                        </svg>
	                    </a>
	                    <span v-else>
	                        <span class="relative inline-flex items-center px-2 py-2 -ml-px text-sm font-medium text-gray-400 bg-white border border-gray-300 cursor-default rounded-r-md leading-5">
	                            <svg class="w-5 h-5 -mr-1" fill="currentColor" viewBox="0 0 20 20">
	                                <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
	                            </svg>
	                            <svg class="w-5 h-5 -ml-2" fill="currentColor" viewBox="0 0 20 20">
	                                <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
	                            </svg>
	                        </span>
	                    </span>
	                    <!-- End Last Page Button -->
	                </span>
	            </div>
	        </div>
        </nav>
        <!-- End Pagination Nav -->

	</div>
</template>

<script>
	export default {
		data: function() {
			return {
				url: 'https://example.com/api/users', /*URL that returns paginated results */
				custom_url: [],
				items: [],		
				from: 0,
				to: 0,
				total: 0,
				current_page: 0,
				last_page: 0,
				first_page_url: [],
				last_page_url: [],
				prev_page_url: [],
				next_page_url: [],
			}
		},
		mounted() {
			this.loadItems(this.url);
		},
		methods: {
			loadItems: function(url) {
				axios.get(url)
					.then((response) => {
						this.readItems(response.data);
					})
					.catch(function(error) {
						console.log(error);
					})
			},
			readItems: function(items) {
				this.custom_url = this.url+'?page=';
				this.items = items.data;
				this.from = items.from;
				this.to = items.to;
				this.total = items.total;
				this.current_page = items.current_page;
				this.last_page = items.last_page;
				this.first_page_url = items.first_page_url;
				this.last_page_url = items.last_page_url;
				this.prev_page_url = items.prev_page_url;
				this.next_page_url = items.next_page_url;
			},
		}
	}
</script>