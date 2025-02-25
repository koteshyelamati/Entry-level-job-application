# Entry-level-job-application
/** @jsxImportSource https://esm.sh/react@18.2.0 */
import React, { useState, useEffect } from "https://esm.sh/react@18.2.0";
import { createRoot } from "https://esm.sh/react-dom@18.2.0/client";

interface JobApplication {
  id: string;
  company: string;
  position: string;
  status: 'Applied' | 'Interviewing' | 'Offer' | 'Rejected';
  dateApplied: string;
  notes?: string;
}

function JobApplicationTracker() {
  const [applications, setApplications] = useState<JobApplication[]>([]);
  const [newApplication, setNewApplication] = useState({
    company: '',
    position: '',
    status: 'Applied' as JobApplication['status']
  });

  const addApplication = (e: React.FormEvent) => {
    e.preventDefault();
    const application: JobApplication = {
      id: Date.now().toString(),
      ...newApplication,
      dateApplied: new Date().toISOString().split('T')[0]
    };

    setApplications([...applications, application]);
    setNewApplication({ company: '', position: '', status: 'Applied' });
  };

  const updateApplicationStatus = (id: string, newStatus: JobApplication['status']) => {
    setApplications(applications.map(app => 
      app.id === id ? { ...app, status: newStatus } : app
    ));
  };

  const deleteApplication = (id: string) => {
    setApplications(applications.filter(app => app.id !== id));
  };

  return (
    <div className="job-application-tracker">
      <h1>🚀 Entry Level Job Application Tracker</h1>
      
      <form onSubmit={addApplication}>
        <input 
          type="text" 
          placeholder="Company Name" 
          value={newApplication.company}
          onChange={(e) => setNewApplication({
            ...newApplication, 
            company: e.target.value
          })}
          required 
        />
        <input 
          type="text" 
          placeholder="Position" 
          value={newApplication.position}
          onChange={(e) => setNewApplication({
            ...newApplication, 
            position: e.target.value
          })}
          required 
        />
        <select 
          value={newApplication.status}
          onChange={(e) => setNewApplication({
            ...newApplication, 
            status: e.target.value as JobApplication['status']
          })}
        >
          <option value="Applied">Applied</option>
          <option value="Interviewing">Interviewing</option>
          <option value="Offer">Offer</option>
          <option value="Rejected">Rejected</option>
        </select>
        <button type="submit">Add Application</button>
      </form>

      <div className="application-list">
        {applications.map(app => (
          <div key={app.id} className="application-card">
            <h3>{app.company}</h3>
            <p>{app.position}</p>
            <p>Status: {app.status}</p>
            <p>Applied on: {app.dateApplied}</p>
            <div className="application-actions">
              <select 
                value={app.status}
                onChange={(e) => updateApplicationStatus(
                  app.id, 
                  e.target.value as JobApplication['status']
                )}
              >
                <option value="Applied">Applied</option>
                <option value="Interviewing">Interviewing</option>
                <option value="Offer">Offer</option>
                <option value="Rejected">Rejected</option>
              </select>
              <button onClick={() => deleteApplication(app.id)}>🗑️</button>
            </div>
          </div>
        ))}
      </div>

      <a 
        href={import.meta.url.replace("esm.town", "val.town")} 
        target="_top" 
        style={{color: '#888', fontSize: '0.8em'}}
      >
        View Source
      </a>
    </div>
  );
}

function client() {
  createRoot(document.getElementById("root")).render(<JobApplicationTracker />);
}
if (typeof document !== "undefined") { client(); }

export default async function server(request: Request): Promise<Response> {
  return new Response(`
    <html>
      <head>
        <title>Job Application Tracker</title>
        <style>${css}</style>
      </head>
      <body>
        <div id="root"></div>
        <script src="https://esm.town/v/std/catch"></script>
        <script type="module" src="${import.meta.url}"></script>
      </body>
    </html>
  `, {
    headers: {
      "content-type": "text/html",
    },
  });
}

const css = `
body {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f0f0f0;
}

.job-application-tracker {
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

form {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 20px;
}

input, select {
  flex-grow: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

button {
  padding: 10px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.application-list {
  display: grid;
  gap: 10px;
}

.application-card {
  background-color: #f8f9fa;
  border-radius: 5px;
  padding: 15px;
  position: relative;
}

.application-actions {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}

.application-actions select {
  width: 80%;
  margin-right: 10px;
}

.application-actions button {
  background-color: #dc3545;
  width: 20%;
}
`;
