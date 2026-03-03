import React, { useState, useEffect, useMemo } from 'react';
import { 
  Calendar as CalendarIcon, 
  CheckSquare, 
  FileText, 
  Plus, 
  Trash2, 
  ChevronLeft, 
  ChevronRight, 
  Link as LinkIcon,
  X,
  Check
} from 'lucide-react';

// --- Components ---

const Sidebar = ({ activeTab, setActiveTab }) => {
  const tabs = [
    { id: 'calendar', icon: CalendarIcon, label: 'Calendar' },
    { id: 'tasks', icon: CheckSquare, label: 'Tasks' },
    { id: 'notes', icon: FileText, label: 'Notes' },
  ];

  return (
    <div className="fixed bottom-0 left-0 right-0 bg-white border-t border-gray-200 flex justify-around p-2 md:relative md:border-t-0 md:border-r md:w-20 md:flex-col md:justify-start md:gap-8 md:pt-8 z-50">
      {tabs.map((tab) => (
        <button
          key={tab.id}
          onClick={() => setActiveTab(tab.id)}
          className={`flex flex-col items-center p-2 rounded-xl transition-all ${
            activeTab === tab.id ? 'text-blue-600 bg-blue-50' : 'text-gray-500 hover:bg-gray-100'
          }`}
        >
          <tab.icon size={24} />
          <span className="text-[10px] mt-1 font-medium">{tab.label}</span>
        </button>
      ))}
    </div>
  );
};

const CalendarTab = () => {
  const [currentDate, setCurrentDate] = useState(new Date());
  const [importUrl, setImportUrl] = useState('');
  const [showImport, setShowImport] = useState(false);

  const daysInMonth = (year, month) => new Date(year, month + 1, 0).getDate();
  const firstDayOfMonth = (year, month) => new Date(year, month, 1).getDay();

  const monthName = currentDate.toLocaleString('default', { month: 'long' });
  const year = currentDate.getFullYear();
  const month = currentDate.getMonth();

  const prevMonth = () => setCurrentDate(new Date(year, month - 1));
  const nextMonth = () => setCurrentDate(new Date(year, month + 1));

  const calendarDays = useMemo(() => {
    const totalDays = daysInMonth(year, month);
    const startOffset = firstDayOfMonth(year, month);
    const days = [];
    // Padding for the start of the month
    for (let i = 0; i < startOffset; i++) {
      days.push({ type: 'padding', value: null, key: `pad-${i}` });
    }
    // Actual days
    for (let i = 1; i <= totalDays; i++) {
      days.push({ type: 'day', value: i, key: `day-${year}-${month}-${i}` });
    }
    return days;
  }, [year, month]);

  const weekHeaders = ['S', 'M', 'T', 'W', 'T', 'F', 'S'];

  return (
    <div className="p-4 animate-in fade-in duration-500">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold text-gray-800">{monthName} {year}</h1>
        <div className="flex gap-2">
          <button onClick={() => setShowImport(true)} className="p-2 text-gray-500 hover:bg-gray-100 rounded-full">
            <LinkIcon size={20} />
          </button>
          <button onClick={prevMonth} className="p-2 text-gray-500 hover:bg-gray-100 rounded-full">
            <ChevronLeft size={20} />
          </button>
          <button onClick={nextMonth} className="p-2 text-gray-500 hover:bg-gray-100 rounded-full">
            <ChevronRight size={20} />
          </button>
        </div>
      </div>

      {showImport && (
        <div className="mb-6 p-4 bg-blue-50 rounded-2xl border border-blue-100 flex gap-2 items-center">
          <input 
            type="text" 
            placeholder="Paste .ics URL (e.g. Google Calendar Secret URL)" 
            className="flex-1 p-2 rounded-lg border border-blue-200 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
            value={importUrl}
            onChange={(e) => setImportUrl(e.target.value)}
          />
          <button onClick={() => setShowImport(false)} className="bg-blue-600 text-white px-4 py-2 rounded-lg text-sm font-medium">
            Import
          </button>
          <button onClick={() => setShowImport(false)} className="text-gray-400 p-1"><X size={18} /></button>
        </div>
      )}

      <div className="grid grid-cols-7 gap-2">
        {weekHeaders.map((d, idx) => (
          <div key={`header-${d}-${idx}`} className="text-center text-xs font-bold text-gray-400 mb-2">{d}</div>
        ))}
        {calendarDays.map((day) => (
          <div 
            key={day.key} 
            className={`aspect-square flex items-center justify-center rounded-xl border text-sm transition-colors ${
              day.value ? 'bg-white border-gray-100 hover:border-blue-300' : 'border-transparent'
            } ${day.value === new Date().getDate() && month === new Date().getMonth() && year === new Date().getFullYear() ? 'border-blue-600 text-blue-600 font-bold bg-blue-50' : ''}`}
          >
            {day.value}
          </div>
        ))}
      </div>
    </div>
  );
};

const TasksTab = () => {
  const [tasks, setTasks] = useState(() => {
    const saved = localStorage.getItem('pwa_tasks');
    return saved ? JSON.parse(saved) : [];
  });
  const [input, setInput] = useState('');

  useEffect(() => {
    localStorage.setItem('pwa_tasks', JSON.stringify(tasks));
  }, [tasks]);

  const addTask = (e) => {
    e.preventDefault();
    if (!input.trim()) return;
    setTasks([{ id: Date.now(), text: input, completed: false }, ...tasks]);
    setInput('');
  };

  const toggleTask = (id) => {
    setTasks(tasks.map(t => t.id === id ? { ...t, completed: !t.completed } : t));
  };

  const deleteTask = (id) => {
    setTasks(tasks.filter(t => t.id !== id));
  };

  return (
    <div className="p-4 max-w-2xl mx-auto">
      <h1 className="text-2xl font-bold text-gray-800 mb-6">Inbox</h1>
      <form onSubmit={addTask} className="mb-6 flex gap-2">
        <input 
          type="text" 
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="What needs to be done?" 
          className="flex-1 p-4 rounded-2xl bg-gray-100 focus:outline-none focus:ring-2 focus:ring-blue-500 border-none shadow-sm"
        />
        <button type="submit" className="bg-blue-600 text-white p-4 rounded-2xl shadow-lg hover:bg-blue-700 transition-transform active:scale-95">
          <Plus size={24} />
        </button>
      </form>

      <div className="space-y-3">
        {tasks.map(task => (
          <div key={task.id} className="group flex items-center gap-4 p-4 bg-white rounded-2xl border border-gray-100 shadow-sm hover:border-blue-200 transition-all">
            <button 
              onClick={() => toggleTask(task.id)}
              className={`w-6 h-6 rounded-full border-2 flex items-center justify-center transition-colors ${
                task.completed ? 'bg-green-500 border-green-500' : 'border-gray-300'
              }`}
            >
              {task.completed && <Check size={14} className="text-white" />}
            </button>
            <span className={`flex-1 text-gray-700 ${task.completed ? 'line-through text-gray-400' : ''}`}>
              {task.text}
            </span>
            <button onClick={() => deleteTask(task.id)} className="text-gray-300 hover:text-red-500 opacity-0 group-hover:opacity-100 transition-opacity">
              <Trash2 size={18} />
            </button>
          </div>
        ))}
      </div>
    </div>
  );
};

const NotesTab = () => {
  const [notes, setNotes] = useState(() => {
    const saved = localStorage.getItem('pwa_notes');
    return saved ? JSON.parse(saved) : [];
  });
  const [isAdding, setIsAdding] = useState(false);
  const [newNote, setNewNote] = useState({ title: '', content: '' });

  useEffect(() => {
    localStorage.setItem('pwa_notes', JSON.stringify(notes));
  }, [notes]);

  const addNote = () => {
    if (!newNote.title && !newNote.content) {
      setIsAdding(false);
      return;
    }
    setNotes([{ id: Date.now(), ...newNote, color: 'bg-white' }, ...notes]);
    setNewNote({ title: '', content: '' });
    setIsAdding(false);
  };

  const deleteNote = (id) => {
    setNotes(notes.filter(n => n.id !== id));
  };

  return (
    <div className="p-4">
      <h1 className="text-2xl font-bold text-gray-800 mb-6">Keep Notes</h1>
      
      {!isAdding ? (
        <div 
          onClick={() => setIsAdding(true)}
          className="mb-8 p-4 bg-white rounded-2xl shadow-sm border border-gray-200 text-gray-400 cursor-text"
        >
          Take a note...
        </div>
      ) : (
        <div className="mb-8 bg-white rounded-2xl shadow-lg border border-gray-200 overflow-hidden animate-in zoom-in-95 duration-200">
          <input 
            autoFocus
            placeholder="Title"
            className="w-full p-4 text-lg font-bold focus:outline-none"
            value={newNote.title}
            onChange={e => setNewNote({...newNote, title: e.target.value})}
          />
          <textarea 
            placeholder="Note contents..."
            className="w-full p-4 h-32 focus:outline-none resize-none"
            value={newNote.content}
            onChange={e => setNewNote({...newNote, content: e.target.value})}
          />
          <div className="p-2 border-t flex justify-end">
            <button onClick={addNote} className="px-6 py-2 text-blue-600 font-bold hover:bg-blue-50 rounded-lg">Close</button>
          </div>
        </div>
      )}

      <div className="columns-2 md:columns-3 lg:columns-4 gap-4 space-y-4">
        {notes.map(note => (
          <div key={note.id} className="break-inside-avoid bg-white p-4 rounded-xl border border-gray-200 shadow-sm hover:shadow-md transition-shadow relative group">
            <button 
              onClick={() => deleteNote(note.id)}
              className="absolute top-2 right-2 p-1 text-gray-300 hover:text-red-500 opacity-0 group-hover:opacity-100 transition-opacity"
            >
              <Trash2 size={16} />
            </button>
            {note.title && <h3 className="font-bold text-gray-800 mb-2">{note.title}</h3>}
            <p className="text-sm text-gray-600 whitespace-pre-wrap">{note.content}</p>
          </div>
        ))}
      </div>
    </div>
  );
};

// --- Main App ---

export default function App() {
  const [activeTab, setActiveTab] = useState('calendar');

  return (
    <div className="min-h-screen bg-white md:flex">
      <Sidebar activeTab={activeTab} setActiveTab={setActiveTab} />
      
      <main className="flex-1 pb-20 md:pb-0 h-screen overflow-y-auto bg-slate-50">
        <div className="max-w-4xl mx-auto py-4">
          {activeTab === 'calendar' && <CalendarTab />}
          {activeTab === 'tasks' && <TasksTab />}
          {activeTab === 'notes' && <NotesTab />}
        </div>
      </main>
    </div>
  );
}
