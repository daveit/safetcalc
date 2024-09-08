<script>
	let formData = { length: null, separation: null };
	let accumulatedData = { stp_value_at: null };
	let submissions = [];
	let warningMessage = '';

	let stp_value = 0;
	$: stp_value_at = 0;
	let stp_lh_rh_value = 0;
	$: stp_lh_rh_value_at = 0;
	let stp_e20_ss_value = 0;
	$: stp_e20_ss_value_at = 0;
	let stp_e40_ss_value = 0;
	$: stp_e40_ss_value_at = 0;
	let stp_e50_ss_value = 0;
	$: stp_e50_ss_value_at = 0;
	let stp_e60_ss_value = 0;
	$: stp_e60_ss_value_at = 0;
	let stp_e75_ss_value = 0;
	$: stp_e75_ss_value_at = 0;
	let stp_e90_ss_value = 0;
	$: stp_e90_ss_value_at = 0;
	let stl_10_value = 0;
	$: stl_10_ss_value_at = 0;
	let stg_200_ss_value = 0;
	$: stg_200_ss_value_at = 0;
	let bothChecked = false;
	$: formData.length = 7500;
	$: formData.separation = 140;
	$: bothsides = 1;
	$: if (formData.separation > 29 && formData.length > 999) {
		stp_value = Math.round(formData.length / formData.separation) * bothsides;
		stp_lh_rh_value = 1;
		stp_e20_ss_value = Math.round(formData.length / formData.separation) * 2 * bothsides;
		stp_e40_ss_value = Math.round(formData.length / formData.separation) * 4 * bothsides;
		stp_e50_ss_value = Math.round(formData.length / formData.separation) * 4 * bothsides;
		stp_e60_ss_value = Math.round(formData.length / formData.separation) * 2 * bothsides;
		stp_e75_ss_value = Math.round(formData.length / formData.separation) * 2 * bothsides;
		stp_e90_ss_value = Math.round(formData.length / formData.separation) * 2 * bothsides;
		stl_10_value = (formData.length * bothsides);
		stg_200_ss_value = 6767;
	} else {
		stp_value = '';
		stp_lh_rh_value = '';
		stp_e20_ss_value = '';
		stp_e40_ss_value = '';
		stp_e50_ss_value = '';
		stp_e60_ss_value = '';
		stp_e75_ss_value = '';
		stp_e90_ss_value = '';
		stl_10_value = '';
		stg_200_ss_value = '';
	}
	const setblank = () => {
		formData.length = '';
		formData.separation = '';
		bothsides = 1;
		bothChecked = false;
	};

	/*const resetAll = () => {
		//formData.length = '';
		//formData.separation = '';
		formData.length = 7500;
		formData.separation = 140;
		bothsides = 1;
		bothChecked = false;
		stp_value_at = 0;
		stp_lh_rh_value_at = 0;
		stp_e20_ss_value_at = 0;
		stp_e40_ss_value_at = 0;
		stp_e50_ss_value_at = 0;
		stp_e60_ss_value_at = 0;
		stp_e75_ss_value_at = 0;
		stp_e90_ss_value_at = 0;
		stl_10_ss_value_at = 0;
		stg_200_ss_value_at = 0;
		submissions = [];
	};
*/
	const resetAll = () => {
		formData.length = 7500;
		formData.separation = 140;
		bothsides = 1;
		bothChecked = false;
		stp_value_at = 0;
		stp_lh_rh_value_at = 0;
		stp_e20_ss_value_at = 0;
		stp_e40_ss_value_at = 0;
		stp_e50_ss_value_at = 0;
		stp_e60_ss_value_at = 0;
		stp_e75_ss_value_at = 0;
		stp_e90_ss_value_at = 0;
		stl_10_ss_value_at = 0;
		stg_200_ss_value_at = 0;
		submissions = [];
	};


	const setdefault = () => {
		formData.length = 7500;
		formData.separation = 140;
		bothChecked = false;
	};
	const handleClick = () => {
		if (!bothChecked) {
			bothsides = 2;
		} else {
			bothsides = 1;
		}
	};
	const addToTotal = () => {
		// Check if the required fields are not empty
		if (!formData.length || !formData.separation) {
			// Show a warning message if any field is empty
			warningMessage = 'One or more values cannot be zero!';
		} else {
			stp_value_at += stp_value;
			stp_lh_rh_value_at += stp_lh_rh_value;
			stp_e20_ss_value_at += stp_e20_ss_value;
			stp_e40_ss_value_at += stp_e40_ss_value;
			stp_e50_ss_value_at += stp_e50_ss_value;
			stp_e60_ss_value_at += stp_e60_ss_value;
			stp_e75_ss_value_at += stp_e75_ss_value;
			stp_e90_ss_value_at += stp_e90_ss_value;
			stl_10_ss_value_at += stl_10_value;
			stg_200_ss_value_at += stg_200_ss_value;
			if (bothChecked) {
				bothsides = 2;
			} else {
				bothsides = 1;
			}
			submissions = [
				...submissions,
				{
					length: (formData.length * bothsides),
					separation: formData.separation,
					stp_value,
					stp_lh_rh_value,
					stp_e20_ss_value,
					stp_e40_ss_value,
					stp_e50_ss_value,
					stp_e60_ss_value,
					stp_e75_ss_value,
					stp_e90_ss_value,
					stl_10_value,
					stg_200_ss_value
				}];
			warningMessage = '';
			//bothsides=1;
			//let bothChecked = false;
		}
	};

	function removeSubmission(index) {
		const submission = submissions[index];

		// Subtract the submission's values from accumulated totals
		stp_value_at -= submission.stp_value;
		stp_lh_rh_value_at -= submission.stp_lh_rh_value;
		stp_e20_ss_value_at -= submission.stp_e20_ss_value;
		stp_e40_ss_value_at -= submission.stp_e40_ss_value;
		stp_e50_ss_value_at -= submission.stp_e50_ss_value;
		stp_e60_ss_value_at -= submission.stp_e60_ss_value;
		stp_e75_ss_value_at -= submission.stp_e75_ss_value;
		stp_e90_ss_value_at -= submission.stp_e90_ss_value;
		stl_10_ss_value_at -= submission.stl_10_value;
		stg_200_ss_value_at -= submission.stg_200_ss_value;

		// Remove the submission from the list
		submissions.splice(index, 1);
		submissions = [...submissions]; // Trigger reactivity
	}

</script>
<div>
	<h1 class="pt-4">Safe-T-Products | Utilities</h1>
</div>
<div class="container mx-auto px-4 py-8">
	<div class="md:flex md:space-x-4">
		<div class="md:w-1/2 mb-4 md:mb-0">

			<h2 class="pb-3 font-semibold text-2xl text-center">Conveyer Calculator</h2>

			<div class="w-full flex justify-center px-2 sm:px-0">
				<div class="w-full flex justify-center px-2 sm:px-0">
					<form class="w-full max-w-md mx-auto border-2 border-gray-400 p-4 m-2 sm:m-5 rounded-lg">
						<div class="space-y-4">
							<div class="flex flex-col items-center">
								<label for="length" class="block text-sm font-medium text-gray-900 mb-2 dark:text-gray-300">
									Enter Length
								</label>
								<input
									bind:value={formData.length}
									type="number"
									id="length"
									class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-32 sm:w-48 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
								/>
							</div>

							<div class="flex flex-col items-center">
								<label for="separation" class="block text-sm font-medium text-gray-900 mb-2 dark:text-gray-300">
									Enter Separation
								</label>
								<input
									bind:value={formData.separation}
									type="number"
									id="separation"
									class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-32 sm:w-48 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
								/>
							</div>

							<div class="flex justify-center items-center">
								<input
									id="purple-checkbox"
									type="checkbox"
									on:click={handleClick}
									bind:checked={bothChecked}
									class="w-4 h-4 text-purple-600 bg-gray-100 border-gray-300 rounded focus:ring-purple-500 dark:focus:ring-purple-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600"
								/>
								<label for="purple-checkbox" class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">
									Both Sides of Belt
								</label>
							</div>
						</div>
					</form>
				</div>
			</div>
<!--			<form class="form-width mx-auto border-2 border-gray-400 p-2 pb-0 m-5">
				<div class="mb-1 mx-auto pb-2">
					<label for="length" class="text-sm font-medium text-gray-900 block mb-2 dark:text-gray-300"
					>Enter Length</label
					>
					<input
						bind:value={formData.length}
						type="number"
						id="length"
						class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
					/>
					<label
						for="Separation"
						class="pt-4 text-sm font-medium text-gray-900 block mb-2 dark:text-gray-300"
					>Enter Separation</label
					>
					<input
						bind:value={formData.separation}
						type="number"
						id="separation"
						class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
					/>
					<div class="flex items-center me-0 my-2 pb-0.5">
						<input id="purple-checkbox" type="checkbox" on:click={handleClick} bind:checked={bothChecked} value=""
									 class="w-4 h-4 text-purple-600 bg-gray-100 border-gray-300 rounded focus:ring-purple-500 dark:focus:ring-purple-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
						<label for="purple-checkbox" class="ms-2 text-sm font-medium text-gray-900 dark:text-gray-300">Both Sides of
							Belt</label>
					</div>

				</div>
			</form>-->
			<!-- Warning Message -->
			{#if warningMessage}
				<h2 class="warning-message">{warningMessage}</h2>
			{/if}

			<div class="pb-5 flex flex-wrap justify-center items-center gap-2 sm:gap-4">
				<button
					on:click={setdefault}
					class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-3 sm:px-4 rounded text-sm sm:text-base whitespace-nowrap"
				>
					Default
				</button>
				<button
					on:click={setblank}
					class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-3 sm:px-4 rounded text-sm sm:text-base whitespace-nowrap"
				>
					Clear Current
				</button>
				<button
					on:click={resetAll}
					class="bg-red-700 hover:bg-red-900 text-white font-bold py-2 px-3 sm:px-4 rounded text-sm sm:text-base whitespace-nowrap"
				>
					Reset ALL
				</button>
				<button
					on:click={addToTotal}
					class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-3 sm:px-4 rounded text-sm sm:text-base whitespace-nowrap"
				>
					Add to Total
				</button>
				<button
					class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-3 sm:px-4 rounded inline-flex items-center text-sm sm:text-base whitespace-nowrap"
				>
					<svg class="fill-current w-3 h-3 sm:w-4 sm:h-4 mr-1 sm:mr-2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
						<path d="M13 8V2H7v6H2l8 8 8-8h-5zM0 18h20v2H0v-2z" />
					</svg>
					<span>Export</span>
				</button>
			</div>

			<div class="container-results mx-auto">
				<h1 class="pb-3 font-semibold text-2xl border-gray-400 border-bottom">Results</h1>
				<div class="align-results">
					<h2 class="text-gray-600 font-bold text-1xl">STP Dual side switch, includes flag</h2>
					<h2 class="text-gray-600 font-bold text-1xl">{stp_value}</h2>
				</div>
				<div class="align-results">
					<h2 class="text-gray-600 font-bold text-1xl">STP-LH/RH Single side switch, includes flag</h2>
					<h2 class="text-gray-600 font-bold text-1xl">{stp_lh_rh_value}</h2>
				</div>
				<div class="align-results">
					<h2 class="text-gray-600 font-bold text-1xl">STP-20-SS Turnbuckle</h2>
					<h2 class="text-gray-600 font-bold text-1xl">{stp_e20_ss_value}</h2>
				</div>
				<div class="align-results">
					<h2 class="text-gray-600 font-bold text-1xl">STP-40-SS Rope thimble</h2>
					<h2 class="text-gray-600 font-bold text-1xl">{stp_e40_ss_value}</h2>
				</div>
				<div class="align-results">
					<h2 class="text-gray-600 font-bold text-1xl">STP-50-SS Horse shoe rope grip</h2>
					<h2 class="text-gray-600 font-bold text-1xl">{stp_e50_ss_value}</h2>
				</div>
				<div class="align-results">
					<h2 class="text-gray-600 font-bold text-1xl">STP-E60 Compensation spring</h2>
					<h2 class="text-gray-600 font-bold text-1xl">{stp_e60_ss_value}</h2>
				</div>
				<div class="align-results">
					<h2 class="text-gray-600 font-bold text-1xl">STP-75-SS Shackle</h2>
					<h2 class="text-gray-600 font-bold text-1xl">{stp_e75_ss_value}</h2>
				</div>
				<div class="align-results">
					<h2 class="text-gray-600 font-bold text-1xl">STP-90-SS Pigtail</h2>
					<h2 class="text-gray-600 font-bold text-1xl">{stp_e90_ss_value}</h2>
				</div>
				<div class="align-results">
					<h2 class="text-gray-600 font-bold text-1xl">STL-10 Wire rope SS or V (metres)</h2>
					<h2 class="text-gray-600 font-bold text-1xl">{stl_10_value}</h2>
				</div>
				<div class="align-results">
					<h2 class="text-gray-600 font-bold text-1xl">STG-200-SS Wire Support</h2>
					<h2 class="text-gray-600 font-bold text-1xl">{stg_200_ss_value}</h2>
				</div>
			</div>


		</div>

			<div class="md:w-1/2">
					<div class="container-results mx-auto mt-2 mb-16">
						<h2 class="pb-3 font-semibold text-2xl mt-1">Accumulated Totals</h2>
						<div class="align-results">
							<h2 class="text-gray-600 font-bold text-1xl">STP Dual side switch, includes flag</h2>
							<h2 class="text-gray-600 font-bold text-1xl">{stp_value_at !== 0 ? stp_value_at : ''}</h2>
						</div>
						<div class="align-results">
							<h2 class="text-gray-600 font-bold text-1xl">STP-LH/RH Single side switch, includes flag</h2>
							<h2 class="text-gray-600 font-bold text-1xl">{stp_lh_rh_value_at !== 0 ? stp_lh_rh_value_at : ''}</h2>
						</div>
						<div class="align-results">
							<h2 class="text-gray-600 font-bold text-1xl">STP-20-SS Turnbuckle</h2>
							<h2 class="text-gray-600 font-bold text-1xl">{stp_e20_ss_value_at !== 0 ? stp_e20_ss_value_at : ''}</h2>
						</div>
						<div class="align-results">
							<h2 class="text-gray-600 font-bold text-1xl">STP-40-SS Rope thimble</h2>
							<h2 class="text-gray-600 font-bold text-1xl">{stp_e40_ss_value_at !== 0 ? stp_e40_ss_value_at : ''}</h2>
						</div>
						<div class="align-results">
							<h2 class="text-gray-600 font-bold text-1xl">STP-50-SS Horse shoe rope grip</h2>
							<h2 class="text-gray-600 font-bold text-1xl">{stp_e50_ss_value_at !== 0 ? stp_e50_ss_value_at : ''}</h2>
						</div>
						<div class="align-results">
							<h2 class="text-gray-600 font-bold text-1xl">STP-E60 Compensation spring</h2>
							<h2 class="text-gray-600 font-bold text-1xl">{stp_e60_ss_value_at !== 0 ? stp_e60_ss_value_at : ''}</h2>
						</div>
						<div class="align-results">
							<h2 class="text-gray-600 font-bold text-1xl">STP-75-SS Shackle</h2>
							<h2 class="text-gray-600 font-bold text-1xl">{stp_e75_ss_value_at !== 0 ? stp_e75_ss_value_at : ''}</h2>
						</div>
						<div class="align-results">
							<h2 class="text-gray-600 font-bold text-1xl">STP-90-SS Pigtail</h2>
							<h2 class="text-gray-600 font-bold text-1xl">{stp_e90_ss_value_at !== 0 ? stp_e90_ss_value_at : ''}</h2>
						</div>
						<div class="align-results">
							<h2 class="text-gray-600 font-bold text-1xl">STL-10 Wire rope SS or V (metres)</h2>
							<h2 class="text-gray-600 font-bold text-1xl">{stl_10_ss_value_at !== 0 ? stl_10_ss_value_at : ''}</h2>
						</div>
						<div class="align-results">
							<h2 class="text-gray-600 font-bold text-1xl">STG-200-SS Wire Support</h2>
							<h2 class="text-gray-600 font-bold text-1xl">{stg_200_ss_value_at !== 0 ? stg_200_ss_value_at : ''}</h2>
						</div>
					</div>

				<div class="container-results mx-auto mt-2 mb-16">
					<h2 class="pb-3 font-semibold text-2xl text-left">Accumulated Entries</h2>

					<ul>
						{#each submissions as submission, index}
							<li>
								{index + 1}: Length: {submission.length}, Separation: {submission.separation}
								<!-- Link to remove the submission -->
								<!-- Link to remove the submission -->
								<button on:click={() => removeSubmission(index)}>❌</button>
							</li>
						{/each}
					</ul>
					<!--{#each $calcStore as calcItem, index (calcItem.thelength)}
						<div>
							<span class="text-gray-600 text-1xl">
								Length: {calcItem.thelength} Separation: {calcItem.theseparation} Both sides of the belt: {calcItem.thebelt}
							</span>
							<span>❌</span>
						</div>
					{/each}-->
				</div>
				</div>

		</div>
	</div>

	<style>
      /*
			.form-width {
					width: 25%;
					align-content: center;
			}

					.cb-padding {
							margin-left: 20px;
					}
			.content {
					width: 75%;
					max-width: var(--column-width);
					margin: var(--column-margin-top) auto 0 auto;
			}
*/

      .container-results {
          width: 75%;
          align-content: center;
      }

      .align-results {
          display: flex;
          justify-content: space-between;
      }

      /*    .align-checkbox {
							display: flex;
							justify-content: center;
					}*/
      h2 {
          text-align: center;
      }

      .warning-message {
          text-align: center;
          font-size: 28px;
          font-weight: bold;
          color: darkred;
          padding-bottom: 10px;
      }
	</style>
