# Clinic_System
Admin.java/com.controller
package com.controller;

import java.io.IOException;
import java.util.ArrayList;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.beans.Patient;
import com.model.Account;

public class Admin extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String action = request.getParameter("action");
		if(action== null){
			request.getRequestDispatcher("admin_jsps/index.jsp").forward(request, response);
		}
		else{ 
			doPost(request,response);
		}
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String action = request.getParameter("action"); //search
		ArrayList<Patient> list = new ArrayList<Patient>();
		if(action.equals("search")){
			String search_query = request.getParameter("search_query");
			if(search_query.equals("")){
				request.setAttribute("msg", "Enter the search query");
				request.setAttribute("flag", 144);
				request.getRequestDispatcher("admin_jsps/index.jsp").forward(request, response);
			}
			else{
			Account account = new Account();
			try {
				list = account.searchPatient(search_query);
			} catch (Exception e) {
				e.printStackTrace();
			}
			request.setAttribute("list", list);
			request.setAttribute("flag", 100);
			request.getRequestDispatcher("admin_jsps/index.jsp").forward(request, response);
		 }
		}
		if(action.equals("new_patient")){
			request.getRequestDispatcher("admin_jsps/new_patient.jsp").forward(request, response);
		}
		
		if(action.equals("book_appt_direct")){
			String id = request.getParameter("id");
			Account account =   new Account();
			Patient p=new Patient();
			try {
				p = account.fetchPatient(id);
			} catch (Exception e) {
				e.printStackTrace();
			}
			String name = p.getName();
			String email = p.getEmail();
			String contact = p.getContact();
			String address = p.getAddress();
			String age = p.getAge();
			request.setAttribute("name", name);
			request.setAttribute("email", email);
			request.setAttribute("contact", contact);
			request.setAttribute("address", address);
			request.setAttribute("age", age); 
			request.getRequestDispatcher("admin_jsps/new_patient.jsp").forward(request, response);
		}
	}
}







