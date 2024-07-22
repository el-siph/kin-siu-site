<script>
	const FORM_ACCESS_KEY = import.meta.env.VITE_FORM_ACCESS_KEY;
	let form;
	let result;

	function onSubmit(e) {
		e.preventDefault();
		const formData = new FormData(form);
		const object = Object.fromEntries(formData);
		const json = JSON.stringify(object);
		result.innerHTML = 'Please wait...';

		fetch('https://api.web3forms.com/submit', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
				Accept: 'application/json'
			},
			body: json
		})
			.then(async (response) => {
				let json = await response.json();
				if (response.status == 200) {
					result.innerHTML = json.message;
				} else {
					console.log(response);
					result.innerHTML = json.message;
				}
			})
			.catch((error) => {
				console.log(error);
				result.innerHTML = 'Something went wrong!';
			})
			.then(function () {
				form.reset();
				setTimeout(() => {
					result.style.display = 'none';
				}, 3000);
			});
	}

	const isNumericInput = (event) => {
		const key = event.keyCode;
		return (
			(key >= 48 && key <= 57) || // Allow number line
			(key >= 96 && key <= 105) // Allow number pad
		);
	};

	const isModifierKey = (event) => {
		const key = event.keyCode;
		return (
			event.shiftKey === true ||
			key === 35 ||
			key === 36 || // Allow Shift, Home, End
			key === 8 ||
			key === 9 ||
			key === 13 ||
			key === 46 || // Allow Backspace, Tab, Enter, Delete
			(key > 36 && key < 41) || // Allow left, up, right, down
			// Allow Ctrl/Command + A,C,V,X,Z
			((event.ctrlKey === true || event.metaKey === true) &&
				(key === 65 || key === 67 || key === 86 || key === 88 || key === 90))
		);
	};

	const enforceFormat = (event) => {
		// Input must be of a valid number format or a modifier key, and not longer than ten digits
		if (!isNumericInput(event) && !isModifierKey(event)) {
			event.preventDefault();
		}
	};

	const formatToPhone = (event) => {
		if (isModifierKey(event)) {
			return;
		}

		const input = event.target.value.replace(/\D/g, '').substring(0, 10); // First ten digits of input only
		const areaCode = input.substring(0, 3);
		const middle = input.substring(3, 6);
		const last = input.substring(6, 10);

		if (input.length > 6) {
			event.target.value = `(${areaCode}) ${middle} - ${last}`;
		} else if (input.length > 3) {
			event.target.value = `(${areaCode}) ${middle}`;
		} else if (input.length > 0) {
			event.target.value = `(${areaCode}`;
		}
	};

	let inputElement;
</script>

<form class="max-w-2xl mx-auto mt-10" method="POST" id="form" bind:this={form} on:submit={onSubmit}>
	<input type="hidden" name="access_key" value={FORM_ACCESS_KEY} />
	<input type="hidden" name="subject" value="New Inquery" />
	<input type="checkbox" name="botcheck" id="" style="display: none;" />

	<div class="relative z-0 w-full mb-5 group">
		<input
			type="email"
			name="Email address"
			id="floating_email"
			class="block py-2.5 px-0 w-full text-sm text-gray-900 bg-transparent border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer"
			placeholder=" "
			required
		/>
		<label
			for="floating_email"
			class="peer-focus:font-medium absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-6 scale-75 top-3 -z-10 origin-[0] peer-focus:start-0 rtl:peer-focus:translate-x-1/4 rtl:peer-focus:left-auto peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-6"
			>Email address</label
		>
	</div>
	<div class="grid md:grid-cols-2 md:gap-6">
		<div class="relative z-0 w-full mb-5 group">
			<input
				type="text"
				name="First name"
				id="floating_first_name"
				class="block py-2.5 px-0 w-full text-sm text-gray-900 bg-transparent border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer"
				placeholder=" "
				required
			/>
			<label
				for="floating_first_name"
				class="peer-focus:font-medium absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-6 scale-75 top-3 -z-10 origin-[0] peer-focus:start-0 rtl:peer-focus:translate-x-1/4 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-6"
				>First name</label
			>
		</div>
		<div class="relative z-0 w-full mb-5 group">
			<input
				type="text"
				name="Last name"
				id="floating_last_name"
				class="block py-2.5 px-0 w-full text-sm text-gray-900 bg-transparent border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer"
				placeholder=" "
				required
			/>
			<label
				for="floating_last_name"
				class="peer-focus:font-medium absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-6 scale-75 top-3 -z-10 origin-[0] peer-focus:start-0 rtl:peer-focus:translate-x-1/4 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-6"
				>Last name</label
			>
		</div>
	</div>
	<div class="grid md:grid-cols-2 md:gap-6">
		<div class="relative z-0 w-full mb-5 group">
			<input
				bind:this={inputElement}
				on:keydown={enforceFormat}
				on:keyup={formatToPhone}
				type="tel"
				name="Phone number"
				id="floating_phone"
				class="block py-2.5 px-0 w-full text-sm text-gray-900 bg-transparent border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer"
				placeholder=" "
				required
			/>
			<label
				for="floating_phone"
				class="peer-focus:font-medium absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-6 scale-75 top-3 -z-10 origin-[0] peer-focus:start-0 rtl:peer-focus:translate-x-1/4 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-6"
				>Phone number</label
			>
		</div>
		<div class="relative z-0 w-full mb-5 group">
			<input
				type="text"
				name="Company name"
				id="floating_company"
				class="block py-2.5 px-0 w-full text-sm text-gray-900 bg-transparent border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer"
				placeholder=" "
			/>
			<label
				for="floating_company"
				class="peer-focus:font-medium absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-6 scale-75 top-3 -z-10 origin-[0] peer-focus:start-0 rtl:peer-focus:translate-x-1/4 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-6"
				>Company (optional)</label
			>
		</div>
	</div>

	<div class="relative z-0 w-full mb-5 group">
		<div class="relative z-0 w-full mb-5 group">
			<textarea
				id="message"
				rows="4"
				name="Message"
				class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
				placeholder="Your Message..."
			></textarea>
		</div>
	</div>
	<button
		type="submit"
		class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
		>Submit</button
	>
	<div id="result" bind:this={result}></div>
</form>
