### google sheets : Form Data  ,page1
Name	Email	Message

### App Script : FormData Script

 const sheets = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1YshqlxctqbJ5THuNGFDUkH3XMLPZAB2frENYE72X2uM/edit?gid=0#gid=0");
 //if you have changed your sheet name then replace the below Sheet1 with your sheet name
const sheet = sheets.getSheetByName("Page1");
function doPost(e){
  let data = e.parameter;
  sheet.appendRow([data.Name,data.Email,data.Message]);
  return ContentService.createTextOutput("Your message was successfully sent to the Googlesheet database!");
}








###react/.tsx code


"use client";
import React, { useState }  from "react";

export default function App() {
  const [status, setStatus] = useState<string>(""); // To handle submission status

  // Function to handle form submission
  function Submit(e: React.FormEvent<HTMLFormElement>) {
    e.preventDefault();
    setStatus("loading"); // Set status to loading before form submission

    const formEle = e.currentTarget; // Use the event to get the form element
    const formData = new FormData(formEle);

    fetch("https://script.google.com/macros/s/AKfycbznuPPRN2k4RBx-CYUmKg2qjKE1Lr-lj1ycgYEEZJ_U0TK1Attt405UN5aSlINJkp3c/exec",
      {
        method: "POST",
        body: formData,
      }
    )
      .then((res) => {
        if (res.ok) {
          // If the response is successful (status 200-299)
          setStatus("success");
          formEle.reset(); // Clear form fields after successful submission
          return res.text(); // Return text in case it's not JSON
        } else {
          setStatus("error");
          throw new Error("Network response was not ok.");
        }
      })
      .then((data) => {
        console.log("Response received:", data);
      })
      .catch((error) => {
        console.error("There was an error!", error);
        setStatus("error"); // Handle the error case
      });
  }

  return (
    <div className="min-h-screen bg-gray-900 flex flex-col items-center justify-center p-6">
      <h1 className="text-3xl font-bold text-white mb-4">Contact Us</h1>
      <h2 className="text-lg text-gray-300 mb-8">Testing......</h2>
      <div className="w-full max-w-md">
        <form
          className="bg-gray-800 p-6 rounded-lg shadow-lg space-y-4"
          onSubmit={Submit} // TSX way of handling form submission
        >
          <input
            className="w-full p-3 rounded-md bg-gray-700 text-gray-300 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="Your Name"
            name="Name"
            type="text"
            required
          />
          <input
            className="w-full p-3 rounded-md bg-gray-700 text-gray-300 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="Your Email"
            name="Email"
            type="email"
            required
          />
          <textarea
            className="w-full p-3 rounded-md bg-gray-700 text-gray-300 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="Your Message"
            name="Message"
            rows={4}
            required
          ></textarea>
          <button
            className={`w-full bg-blue-600 text-white p-3 rounded-md font-semibold hover:bg-blue-700 transition duration-300 ${
              status === "loading" ? "opacity-50 cursor-not-allowed" : ""
            }`}
            type="submit"
            disabled={status === "loading"} // Disable button while submitting
          >
            {status === "loading" ? "Submitting..." : "Submit"}
          </button>
        </form>

        {/* Conditional rendering for success message */}
        {status === "success" && (
          <div className="mt-4 text-green-500 font-semibold">
            Successfully submitted!
          </div>
        )}

        {status === "error" && (
          <div className="mt-4 text-red-500 font-semibold">
            Error submitting the form. Please try again.
          </div>
        )}
      </div>
    </div>
  );
}




===========================================
##google sheet :
filename : FormData_Atenas || Page1 || copy url --> for App Script 

Name	Email	Phone	Subject	Message	
===========================================
##App Script
filename:FormDataScript_Atenas || Deployment done anyone ||copy Url for the next----->react form

const sheets = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1FaH2dYJ5RXaaU8Lg_IjsWAx6KxKp99twcw4RSBsgJmc/edit?gid=0#gid=0");
 //if you have changed your sheet name then replace the below Sheet1 with your sheet name
const sheet = sheets.getSheetByName("Page1");
function doPost(e){
  let data = e.parameter;
  sheet.appendRow([data.Name,data.Email,data.Phone,data.Subject,data.Message]);
  return ContentService.createTextOutput("Your message was successfully sent to the Googlesheet database!");
}
========================================
##rect(page.tsx)

"use client";
import React, { useState } from "react";
import Card from "@/components/ContactUsPage/Card";
import { IoLocationOutline } from "react-icons/io5";
import { MdPhoneInTalk } from "react-icons/md";
import { RiMailSendLine } from "react-icons/ri";

export default function Page() {
	const [formData, setFormData] = useState({
		name: "",
		email: "",
		phone: "",
		subject: "",
		message: "",
	});
	
	const [status, setStatus] = useState<string>(""); // To handle submission status

	// Function to handle form input changes
	const handleChange = (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
		const { id, value } = e.target;
		setFormData((prevData) => ({
			...prevData,
			[id]: value,
		}));
	};

	// Function to handle form submission
	const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
		e.preventDefault();
		setStatus("Submitting.."); // Set status to loading before form submission

		const formEle = e.currentTarget; // Use the event to get the form element
		const formDataToSend = new FormData(formEle);

		// Append form data keys to match Google Sheets headers
		formDataToSend.append("Name", formData.name);
		formDataToSend.append("Email", formData.email);
		formDataToSend.append("Phone", formData.phone);
		formDataToSend.append("Subject", formData.subject);
		formDataToSend.append("Message", formData.message);

		fetch("https://script.google.com/macros/s/AKfycby8TuIaqNCf5gxSjJ9_nGCjMr1l3My1qHuZ80Tgm2sQ6sPUHjJOsLXkCU9JquMcJPdi/exec", {
			method: "POST",
			body: formDataToSend,
		})
			.then((res) => {
				if (res.ok) {
					setStatus("Successfully submitted!");
					// Reset form data to default state
					setFormData({
						name: "",
						email: "",
						phone: "",
						subject: "",
						message: "",
					});
					return res.text(); // Return text in case it's not JSON
				} else {
					setStatus("Error occurred");
					throw new Error("Network response was not ok.");
				}
			})
			.then((data) => {
				console.log("Response received:", data);
			})
			.catch((error) => {
				console.error("There was an error!", error);
				setStatus("Error occurred"); // Handle the error case
			});
	};

	return (
		<div className="bg-white min-h-screen">
			<section>
				<div className="relative h-[60svh] bg-[url('/images/flight.jpg')] bg-no-repeat bg-cover bg-center flex flex-col justify-center items-center gap-5 text-white">
					<div className="absolute inset-0 bg-black opacity-35 z-0"></div>
					<h1 className="text-5xl font-bold z-10">Contact Us</h1>
					<h3 className="text-lg z-10">Home - Contact Us</h3>
				</div>
				<div className="flex flex-wrap gap-5 justify-center items-center relative bottom-10 p-5">
					<Card
						Icon={IoLocationOutline}
						title="Andheri – Headquarters"
						description="201-A, 2nd Floor, Sunteck Grandeur, Opp Andheri Subway, Swami Vivekananda Road, Andheri West, Maharashtra 400058."
					/>
					<Card
						Icon={IoLocationOutline}
						title="Hyderabad"
						description="2nd Floor, Prime Plaza, Himayat Nagar, AP State Housing Board, Himayatnagar, Hyderabad, Telangana 500029"
					/>
					<Card
						Icon={MdPhoneInTalk}
						title="Support"
						description="+91 8422 8499 85"
					/>
					<Card
						Icon={RiMailSendLine}
						title="Email"
						description="info@atenas.in"
					/>
				</div>
			</section>

			<section className="flex flex-col justify-center items-center mt-5 mb-20">
				<div className="text-center my-5">
					<h3 className="text-2xl text-red-500">-Get in Touch-</h3>
					<h1 className="text-4xl">Let&apos;s Get in Touch</h1>
				</div>

				<div className="w-full max-w-[1200px] flex flex-col md:flex-row justify-center items-center gap-5 mt-10 px-5">
					<div className="w-full md:w-1/2">
						<form onSubmit={handleSubmit} className="h-auto max-h-[600px] flex flex-col justify-between text-black">
							<div className="flex flex-col mb-4">
								<label htmlFor="name" className="text-xl">
									Your name (required)
								</label>
								<input
									type="text"
									id="name"
									value={formData.name}
									onChange={handleChange}
									className="border border-neutral-400 h-12 outline-none px-2"
								/>
							</div>

							<div className="flex gap-x-2 mb-4">
								<div className="flex flex-col w-1/2">
									<label htmlFor="email" className="text-xl">
										Your email (required)
									</label>
									<input
										type="email"
										id="email"
										value={formData.email}
										onChange={handleChange}
										className="border border-neutral-400 h-12 w-full outline-none px-2"
									/>
								</div>

								<div className="flex flex-col w-1/2">
									<label htmlFor="phone" className="text-xl">
										Your Contact No. (required)
									</label>
									<input
										type="text"
										id="phone"
										value={formData.phone}
										onChange={handleChange}
										className="border border-neutral-400 h-12 w-full outline-none px-2"
									/>
								</div>
							</div>

							<div className="flex flex-col mb-4">
								<label htmlFor="subject" className="text-xl">
									Subject
								</label>
								<input
									type="text"
									id="subject"
									value={formData.subject}
									onChange={handleChange}
									className="border border-neutral-400 h-12 outline-none px-2"
								/>
							</div>

							<div className="flex flex-col mb-4">
								<label htmlFor="message" className="text-xl">
									Your message (optional)
								</label>
								<textarea
									id="message"
									cols={40}
									rows={5}
									maxLength={2000}
									value={formData.message}
									onChange={handleChange}
									className="border border-neutral-400 h-40 resize-none outline-none px-2"
								/>
							</div>

							<button
								type="submit"
								className="self-start bg-primary text-white py-2 px-4 mt-2"
							>
								SUBMIT
							</button>
							{status && <p className="mt-4 text-xl text-green-600">{status}</p>}
						</form>
					</div>

					<div className="w-full md:w-1/2 max-h-[900px]">
						<iframe
							src="https://www.google.com/maps/embed?pb=!1m14!1m8!1m3!1d15228.868501373112!2d78.4845403!3d17.4013647!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3bcb9976611386d9%3A0xee3dfe46f76164d2!2sAtenas%20Fraternity!5e0!3m2!1sen!2sin!4v1727964918405!5m2!1sen!2sin"
							loading="lazy"
							className="border-0 h-[80svh] max-h-[900px] w-full"
						></iframe>
					</div>
				</div>
			</section>
		</div>
	);
}
=========================

removing that feedback after 10 sec 
To reset the status message after a successful submission and remove it after a certain period, you can use the `setTimeout` function to clear the status after a few seconds. Here’s how you can implement it:

```javascript
"use client";
import React, { useState } from "react";
import Card from "@/components/ContactUsPage/Card";
import { IoLocationOutline } from "react-icons/io5";
import { MdPhoneInTalk } from "react-icons/md";
import { RiMailSendLine } from "react-icons/ri";

export default function Page() {
	const [formData, setFormData] = useState({
		name: "",
		email: "",
		phone: "",
		subject: "",
		message: "",
	});
	
	const [status, setStatus] = useState<string>(""); // To handle submission status

	// Function to handle form input changes
	const handleChange = (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
		const { id, value } = e.target;
		setFormData((prevData) => ({
			...prevData,
			[id]: value,
		}));
	};

	// Function to handle form submission
	const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
		e.preventDefault();
		setStatus("Submitting.."); // Set status to loading before form submission

		const formEle = e.currentTarget; // Use the event to get the form element
		const formDataToSend = new FormData(formEle);

		// Append form data keys to match Google Sheets headers
		formDataToSend.append("Name", formData.name);
		formDataToSend.append("Email", formData.email);
		formDataToSend.append("Phone", formData.phone);
		formDataToSend.append("Subject", formData.subject);
		formDataToSend.append("Message", formData.message);

		fetch("https://script.google.com/macros/s/AKfycby8TuIaqNCf5gxSjJ9_nGCjMr1l3My1qHuZ80Tgm2sQ6sPUHjJOsLXkCU9JquMcJPdi/exec", {
			method: "POST",
			body: formDataToSend,
		})
			.then((res) => {
				if (res.ok) {
					setStatus("Successfully submitted!");
					// Reset form data to default state
					setFormData({
						name: "",
						email: "",
						phone: "",
						subject: "",
						message: "",
					});
					
					// Clear status message after 3 seconds
					setTimeout(() => {
						setStatus("");
					}, 3000);
					
					return res.text(); // Return text in case it's not JSON
				} else {
					setStatus("Error occurred");
					throw new Error("Network response was not ok.");
				}
			})
			.then((data) => {
				console.log("Response received:", data);
			})
			.catch((error) => {
				console.error("There was an error!", error);
				setStatus("Error occurred"); // Handle the error case
			});
	};

	return (
		<div className="bg-white min-h-screen">
			<section>
				<div className="relative h-[60svh] bg-[url('/images/flight.jpg')] bg-no-repeat bg-cover bg-center flex flex-col justify-center items-center gap-5 text-white">
					<div className="absolute inset-0 bg-black opacity-35 z-0"></div>
					<h1 className="text-5xl font-bold z-10">Contact Us</h1>
					<h3 className="text-lg z-10">Home - Contact Us</h3>
				</div>
				<div className="flex flex-wrap gap-5 justify-center items-center relative bottom-10 p-5">
					<Card
						Icon={IoLocationOutline}
						title="Andheri – Headquarters"
						description="201-A, 2nd Floor, Sunteck Grandeur, Opp Andheri Subway, Swami Vivekananda Road, Andheri West, Maharashtra 400058."
					/>
					<Card
						Icon={IoLocationOutline}
						title="Hyderabad"
						description="2nd Floor, Prime Plaza, Himayat Nagar, AP State Housing Board, Himayatnagar, Hyderabad, Telangana 500029"
					/>
					<Card
						Icon={MdPhoneInTalk}
						title="Support"
						description="+91 8422 8499 85"
					/>
					<Card
						Icon={RiMailSendLine}
						title="Email"
						description="info@atenas.in"
					/>
				</div>
			</section>

			<section className="flex flex-col justify-center items-center mt-5 mb-20">
				<div className="text-center my-5">
					<h3 className="text-2xl text-red-500">-Get in Touch-</h3>
					<h1 className="text-4xl">Let&apos;s Get in Touch</h1>
				</div>

				<div className="w-full max-w-[1200px] flex flex-col md:flex-row justify-center items-center gap-5 mt-10 px-5">
					<div className="w-full md:w-1/2">
						<form onSubmit={handleSubmit} className="h-auto max-h-[600px] flex flex-col justify-between text-black">
							<div className="flex flex-col mb-4">
								<label htmlFor="name" className="text-xl">
									Your name (required)
								</label>
								<input
									type="text"
									id="name"
									value={formData.name}
									onChange={handleChange}
									className="border border-neutral-400 h-12 outline-none px-2"
								/>
							</div>

							<div className="flex gap-x-2 mb-4">
								<div className="flex flex-col w-1/2">
									<label htmlFor="email" className="text-xl">
										Your email (required)
									</label>
									<input
										type="email"
										id="email"
										value={formData.email}
										onChange={handleChange}
										className="border border-neutral-400 h-12 w-full outline-none px-2"
									/>
								</div>

								<div className="flex flex-col w-1/2">
									<label htmlFor="phone" className="text-xl">
										Your Contact No. (required)
									</label>
									<input
										type="text"
										id="phone"
										value={formData.phone}
										onChange={handleChange}
										className="border border-neutral-400 h-12 w-full outline-none px-2"
									/>
								</div>
							</div>

							<div className="flex flex-col mb-4">
								<label htmlFor="subject" className="text-xl">
									Subject
								</label>
								<input
									type="text"
									id="subject"
									value={formData.subject}
									onChange={handleChange}
									className="border border-neutral-400 h-12 outline-none px-2"
								/>
							</div>

							<div className="flex flex-col mb-4">
								<label htmlFor="message" className="text-xl">
									Your message (optional)
								</label>
								<textarea
									id="message"
									cols={40}
									rows={5}
									maxLength={2000}
									value={formData.message}
									onChange={handleChange}
									className="border border-neutral-400 h-40 resize-none outline-none px-2"
								/>
							</div>

							<button
								type="submit"
								className="self-start bg-primary text-white py-2 px-4 mt-2"
							>
								SUBMIT
							</button>
							{status && <p className="mt-4 text-xl text-green-600">{status}</p>}
						</form>
					</div>

					<div className="w-full md:w-1/2 max-h-[900px]">
						<iframe
							src="https://www.google.com/maps/embed?pb=!1m14!1m8!1m3!1d15228.868501373112!2d78.4845403!3d17.4013647!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3bcb9976611386d9%3A0xee3dfe46f76164d2!2sAtenas%20Fraternity!5e0!3m2!1sen!2sin!4v1727964918405!5m2!1sen!2sin"
							loading="lazy"
							className="border-0 h-[80svh] max-h-[900px] w-full"
						></iframe>
					</div>
				</div>
			</section>
		</div>
	);
}
===============================
Name	Email	Phone	CourseSelection	NearestBranch	Timestamp and page 1 ---> Google sheet url----> use it in App script 

## fetching data with time stamp and pop up
===================
=============
============
Here is the corrected and organized version of your code with some improvements:

```tsx
"use client";
import { usePathname } from "next/navigation";
import React, { useState } from "react";
import { FaAnglesRight, FaAnglesLeft } from "react-icons/fa6";

function PopUpForm() {
	const [showForm, setShowForm] = useState(false);
	const [isClosing, setIsClosing] = useState(false);
	const [formData, setFormData] = useState({
		name: "",
		email: "",
		phone: "",
		course: "",
		nearestBranch: "",
	});

	// Handle form close
	const closeForm = () => {
		setIsClosing(true);
		setTimeout(() => {
			setShowForm(false);
			setIsClosing(false);
		}, 900);
	};

	// Handle form open
	const openForm = () => {
		setShowForm(true);
		setIsClosing(false);
	};

	// Handle form input changes
	const handleInputChange = (e: React.ChangeEvent<HTMLInputElement | HTMLSelectElement>) => {
		const { name, value } = e.target;
		setFormData({ ...formData, [name]: value });
	};

	// Handle form submission
	const handleSubmit = async (e: React.FormEvent) => {
		e.preventDefault();

		try {
			const response = await fetch(
				"https://script.google.com/macros/s/AKfycbzjRk7uoeuBaoTM8Ly1RP25KEh1r6POhe6If_tCB-SxbmC-lpoWKtMklXL22k67YCHw/exec",
				{
					method: "POST",
					headers: {
						"Content-Type": "application/x-www-form-urlencoded",
					},
					body: new URLSearchParams({
						Name: formData.name,
						Email: formData.email,
						Phone: formData.phone,
						CourseSelection: formData.course,
						NearestBranch: formData.nearestBranch,
						Timestamp: new Date().toISOString(),
					}).toString(),
				}
			);

			if (response.ok) {
				alert("Enquiry successfully submitted!");
				setFormData({
					name: "",
					email: "",
					phone: "",
					course: "",
					nearestBranch: "",
				});
			} else {
				alert("There was an error submitting your enquiry.");
			}
		} catch (error) {
			console.error("Error submitting the form", error);
			alert("An error occurred while submitting the form.");
		}
	};

	return (
		<div
			className={
				"fixed items-center -right-[375px] bottom-[10%] flex z-20 rounded-lg" +
				(showForm ? " animate-slideIn" : "") +
				(isClosing ? " animate-slideOut" : "")
			}
		>
			<div
				className="flex items-center h-fit gap-x-4 bg-white text-primary font-medium p-3 cursor-pointer text-sm shadow-2xl border-primary border"
				style={{ writingMode: "vertical-rl", textOrientation: "upright" }}
				onClick={showForm ? closeForm : openForm}
			>
				<span>ENQUIRY HERE</span>
				{showForm ? <FaAnglesRight size={15} /> : <FaAnglesLeft size={15} />}
			</div>

			<div className="bg-primary p-6">
				<form className="text-xl flex flex-col gap-y-5" onSubmit={handleSubmit}>
					<h2 className="text-2xl text-white font-medium">Enroll Here for the Course</h2>
					<div>
						<input
							type="text"
							name="name"
							id="name"
							placeholder="Your Name"
							className="p-2 outline-none w-full rounded-md"
							value={formData.name}
							onChange={handleInputChange}
							required
						/>
					</div>
					<div>
						<input
							type="email"
							name="email"
							id="email"
							placeholder="Your Email"
							className="p-2 outline-none w-full rounded-md"
							value={formData.email}
							onChange={handleInputChange}
							required
						/>
					</div>
					<div>
						<input
							type="tel"
							name="phone"
							id="phone"
							placeholder="Your Phone"
							className="p-2 outline-none w-full rounded-md"
							value={formData.phone}
							onChange={handleInputChange}
							required
						/>
					</div>
					<div>
						<select
							name="course"
							id="course"
							className="p-2 outline-none w-full rounded-md"
							value={formData.course}
							onChange={handleInputChange}
							required
						>
							<option value="" disabled>
								Diploma in
							</option>
							<option value="Cabin Crew">Cabin Crew</option>
							<option value="Ground Staff">Ground Staff</option>
							<option value="Hospitality Management">Hospitality Management</option>
							<option value="Travel and Tourism Management">
								Travel and Tourism Management
							</option>
						</select>
					</div>
					<div>
						<select
							name="nearestBranch"
							id="nearest-branch"
							className="p-2 outline-none w-full rounded-md"
							value={formData.nearestBranch}
							onChange={handleInputChange}
							required
						>
							<option value="" disabled>
								Nearest Branch
							</option>
							<option value="Andheri">Andheri</option>
							<option value="Hyderabad">Hyderabad</option>
							<option value="Saki Naka">Saki Naka</option>
							<option value="Vashi">Vashi</option>
						</select>
					</div>
					<div className="flex justify-center">
						<button
							className="mt-3 bg-white text-primary border border-primary hover:bg-primary hover:text-white px-3 py-1 rounded-lg w-[200px] transition-all"
							type="submit"
						>
							Book Now
						</button>
					</div>
				</form>
			</div>
		</div>
	);
}

export default PopUpForm;
```

### Key improvements:
1. **Optimized the `closeForm` function** to simplify the state update sequence.
2. Added **`async` and `await`** for the form submission to handle it properly.
3. Used consistent and semantic code formatting for better readability.







```

### Changes Made:
1. **Reset Status Message**: Added a `setTimeout` function to clear the `status` message after 3 seconds once the form is successfully submitted.
2. **Clear Form Data**: The form data is reset to its default state immediately after successful submission
